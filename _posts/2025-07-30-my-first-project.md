---
layout: post
title: "일정 관리 웹 프로젝트"
category: project
---

## 프로젝트 개요
2학년 1학기 React를 사용한 웹 개발 (개인)프로젝트

## 사용 기술
- HTML, CSS, React

App.js 파일은 아래와 같이 작성함

<details>
<summary>App.js(클릭해서 열기)</summary>
```
import React, { useState, useEffect } from 'react';
import Calendar from 'react-calendar';
import 'react-calendar/dist/Calendar.css';
import Login from './Login';
import Signup from './Signup';
import './App.css';

function App() {
    const [date, setDate] = useState(new Date());
    const [events, setEvents] = useState([]);
    const [eventInput, setEventInput] = useState('');
    const [selectedDate, setSelectedDate] = useState(new Date());
    const [searchInput, setSearchInput] = useState('');
    const [pastSearchInput, setPastSearchInput] = useState('');
    const [isLoggedIn, setIsLoggedIn] = useState(false);
    const [users, setUsers] = useState([]);
    const [currentUser, setCurrentUser] = useState('');
    const [isPastEventsVisible, setIsPastEventsVisible] = useState(false); // 전체일정과 지난일정을 전환하는 상태

    useEffect(() => {
        const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
        const storedUser = localStorage.getItem('currentUser') || '';
        setUsers(storedUsers);
        setCurrentUser(storedUser);
        setIsLoggedIn(!!storedUser);

        if (storedUser) {
            const userEvents = JSON.parse(localStorage.getItem(`events_${storedUser}`)) || [];
            const formattedEvents = userEvents.map(event => ({
                ...event,
                date: new Date(event.date)
            }));
            setEvents(formattedEvents);
        }
    }, []);

    const addEvent = () => {
        if (eventInput) {
            const newEvent = { date, event: eventInput };
            const newEvents = [...events, newEvent];
            setEvents(newEvents);
            setEventInput('');
            localStorage.setItem(`events_${currentUser}`, JSON.stringify(newEvents.map(event => ({
                ...event,
                date: event.date.toISOString()
            }))));
        }
    };

    const deleteEvent = (eventToDelete) => {
        const newEvents = events.filter(event => event !== eventToDelete);
        setEvents(newEvents);
        localStorage.setItem(`events_${currentUser}`, JSON.stringify(newEvents.map(event => ({
            ...event,
            date: event.date.toISOString()
        }))));
    };

    const handleDateClick = (date) => {
        setSelectedDate(date);
        setDate(date);
    };

    const handleEventClick = (eventDate) => {
        setDate(eventDate);
        setSelectedDate(eventDate);
    };

    const handleKeyDown = (e) => {
        if (e.key === 'Enter') {
            addEvent();
        }
    };

    const handleLogin = (username, password) => {
        const user = users.find(user => user.username === username && user.password === password);
        if (user) {
            setIsLoggedIn(true);
            setCurrentUser(username);
            localStorage.setItem('currentUser', username);

            const userEvents = JSON.parse(localStorage.getItem(`events_${username}`)) || [];
            const formattedEvents = userEvents.map(event => ({
                ...event,
                date: new Date(event.date)
            }));
            setEvents(formattedEvents);
        } else {
            alert('잘못된 사용자 이름 또는 비밀번호입니다.');
        }
    };

    const handleSignup = (username, password) => {
        const existingUser = users.find(user => user.username === username);
        if (existingUser) {
            alert('이미 존재하는 사용자 이름입니다. 다른 사용자 이름을 선택하세요.');
            return;
        }

        const newUser = { username, password };
        setUsers([...users, newUser]);
        setIsLoggedIn(true);
        setCurrentUser(username);
        localStorage.setItem('currentUser', username);
        localStorage.setItem(`events_${username}`, JSON.stringify([]));
        localStorage.setItem('users', JSON.stringify([...users, newUser]));
    };

    const handleLogout = () => {
        setIsLoggedIn(false);
        setCurrentUser('');
        localStorage.removeItem('currentUser');
        setEvents([]);
    };

    const formatDate = (date) => {
        const year = date.getFullYear();
        const month = String(date.getMonth() + 1).padStart(2, '0');
        const day = String(date.getDate()).padStart(2, '0');
        const isPast = calculateDaysLeft(date) < 0;
        return (
            <span style={{ 
                fontWeight: 'bold', 
                color: isPast ? '#FF6347' : '#007BFF', 
                marginRight: '10px'
            }}>
                {year}년 {month}월 {day}일
            </span>
        );
    };

    const calculateDaysLeft = (eventDate) => {
        const today = new Date();
        const timeDiff = eventDate - today;
        const daysLeft = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
        return daysLeft;
    };

    const tileContent = ({ date, view }) => {
        const dateString = date.toDateString();
        const eventsForDate = events.filter(event => event.date.toDateString() === dateString);

        const isPastDate = date < new Date() && date.toDateString() !== new Date().toDateString();

        return (
            <div
                style={{
                    backgroundColor: isPastDate ? '#ffcccc' : (eventsForDate.length > 0 ? '#add8e6' : 'transparent'),
                    borderRadius: '4px',
                    padding: '5px',
                    border: '2px solid #000000',
                    boxSizing: 'border-box',
                }}
            >
                {eventsForDate.length > 0 && (
                    <span style={{ fontSize: '13px', color: '#000000' }}>
                        {eventsForDate.length}개 일정
                    </span>
                )}
            </div>
        );
    };

    const pastEvents = events.filter(event => calculateDaysLeft(event.date) < 0);

    const handleToggleView = () => {
        setIsPastEventsVisible(prevState => !prevState); // 전환 버튼을 눌러서 보이는 뷰 변경
    };

    return (
        <div style={{ display: 'flex', justifyContent: 'space-between', width: '100%' }}>
            {!isLoggedIn ? (
                <div className="login-container">
                    <Login onLogin={handleLogin} />
                    <Signup onSignup={handleSignup} />
                </div>
            ) : (
                <>
                    <div className="calendar-container" style={{ flex: 2, marginRight: '10px' }}>
                        <div className="calendar-box">
                            <h1>{currentUser}의 일정</h1>
                            <Calendar 
                                onChange={(date) => { setDate(date); handleDateClick(date); }} 
                                value={date} 
                                tileContent={tileContent}
                            />
                            <div style={{ display: 'flex', alignItems: 'center' }}>
                                <input 
                                    className="input-field"
                                    type="text" 
                                    placeholder="일정 입력" 
                                    value={eventInput} 
                                    onChange={(e) => setEventInput(e.target.value)}
                                    onKeyDown={handleKeyDown}
                                />
                                <button className="button" onClick={addEvent}>추가</button>
                            </div>
                            <ul className="event-list">
                                {events.filter(event => event.date.toDateString() === selectedDate.toDateString()).map((event, index) => (
                                    <li key={index} className="event-item">
                                        {formatDate(event.date)}
                                        <span style={{ color: '#333' }}>
                                            {event.event}
                                        </span>
                                        <span style={{ marginLeft: '10px', color: '#999' }}>
                                            {calculateDaysLeft(event.date) < 0 ? '마감' : `${calculateDaysLeft(event.date)}일 남음`}
                                        </span>
                                        <button onClick={() => deleteEvent(event)}>삭제</button>
                                    </li>
                                ))}
                                {events.length === 0 && (
                                    <li style={{ textAlign: 'center', color: '#999' }}>등록된 일정이 없습니다.</li>
                                )}
                            </ul>
                        </div>
                    </div>

                    <div className="calendar-container" style={{ flex: 1, marginLeft: '10px' }}>
                        <div className="event-box">
                            <h2>{isPastEventsVisible ? '지난 일정 목록' : '전체 일정 목록'}</h2> {/* 제목 동적 변경 */}
                            <button 
                                onClick={handleToggleView} 
                                style={{
                                    backgroundColor: '#007BFF',
                                    color: '#fff',
                                    padding: '10px 20px',
                                    border: 'none',
                                    borderRadius: '5px',
                                    cursor: 'pointer',
                                    transition: 'background-color 0.3s',
                                    marginBottom: '10px'
                                }}
                                onMouseEnter={(e) => e.target.style.backgroundColor = '#0056b3'}
                                onMouseLeave={(e) => e.target.style.backgroundColor = '#007BFF'}
                            >
                                {isPastEventsVisible ? '전체 일정 보기' : '지난 일정 보기'}
                            </button>

                            {isPastEventsVisible ? (
                                <>
                                    <input 
                                        className="input-field"
                                        type="text" 
                                        placeholder="지난 일정 검색" 
                                        value={pastSearchInput}
                                        onChange={(e) => setPastSearchInput(e.target.value)}
                                    />
                                    <ul className="event-list">
                                        {pastEvents.length === 0 ? (
                                            <li style={{ textAlign: 'center', color: '#999' }}>등록된 지난 일정이 없습니다.</li>
                                        ) : (
                                            pastEvents
                                                .filter(event => 
                                                    event.event.toLowerCase().includes(pastSearchInput.toLowerCase())
                                                )
                                                .sort((a, b) => a.date - b.date)
                                                .map((event, index) => (
                                                    <li key={index} className="event-item" onClick={() => handleEventClick(event.date)}>
                                                        {formatDate(event.date)}
                                                        <span style={{ color: '#333' }}>
                                                            {event.event}
                                                        </span>
                                                        <span style={{ marginLeft: '10px', color: '#999' }}>
                                                            마감
                                                        </span>
                                                        <button onClick={() => deleteEvent(event)}>삭제</button>
                                                    </li>
                                                ))
                                        )}
                                    </ul>
                                </>
                            ) : (
                                <>
                                    <input 
                                        className="input-field"
                                        type="text" 
                                        placeholder="일정 검색" 
                                        value={searchInput}
                                        onChange={(e) => setSearchInput(e.target.value)}
                                    />
                                    <ul className="event-list">
                                        {events.length === 0 ? (
                                            <li style={{ textAlign: 'center', color: '#999' }}>등록된 일정이 없습니다.</li>
                                        ) : (
                                            events
                                                .filter(event => 
                                                    event.event.toLowerCase().includes(searchInput.toLowerCase()) && calculateDaysLeft(event.date) >= 0
                                                )
                                                .sort((a, b) => a.date - b.date)
                                                .map((event, index) => (
                                                    <li key={index} className="event-item" onClick={() => handleEventClick(event.date)}>
                                                        {formatDate(event.date)}
                                                        <span style={{ color: '#333' }}>
                                                            {event.event}
                                                        </span>
                                                        <span style={{ marginLeft: '10px', color: '#999' }}>
                                                            {calculateDaysLeft(event.date) < 0 ? '마감' : `${calculateDaysLeft(event.date)}일 남음`}
                                                        </span>
                                                        <button onClick={() => deleteEvent(event)}>삭제</button>
                                                    </li>
                                                ))
                                        )}
                                    </ul>
                                </>
                            )}
                        </div>
                    </div>
                </>
            )}
            {isLoggedIn && (
                <button className="logout-button" onClick={handleLogout}>로그아웃</button>
            )}
        </div>
    );
}

export default App;
```
</details>

App.css파일은 아래처럼 작성함

<details>
<summary>App.css(클릭해서 열기)</summary>
</details>
