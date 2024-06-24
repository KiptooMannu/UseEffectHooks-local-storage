Add a Button to Clear Local Storage
You might want to add a button to clear the local storage for testing purposes or to refresh the data.

Modify the UserList.jsx file to include a button that clears the local storage:

          import React, { useState, useEffect } from 'react';
import './UserList.css';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const storedUsers = localStorage.getItem('users');
    if (storedUsers) {
      setUsers(JSON.parse(storedUsers));
      setLoading(false);
    } else {
      const fetchData = async () => {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          const data = await response.json();
          setUsers(data);
          localStorage.setItem('users', JSON.stringify(data));
          setLoading(false);
        } catch (error) {
          setError(error);
          setLoading(false);
        }
      };

      fetchData();
    }
  }, []);

  const clearLocalStorage = () => {
    localStorage.removeItem('users');
    setUsers([]);
    setLoading(true);
  };

  if (loading) {
    return <div>Loading...</div>;
  }

  if (error) {
    return <div>Error: {error.message}</div>;
  }

  return (
    <div className="user-list">
      <h1>User List</h1>
      <button onClick={clearLocalStorage}>Clear Local Storage</button>
      <ul>
        {users.map(user => (
          <li key={user.id} className="user-card">
            <h2>{user.name}</h2>
            <p><strong>Email:</strong> {user.email}</p>
            <p><strong>Phone:</strong> {user.phone}</p>
            <p><strong>Website:</strong> <a href={`http://${user.website}`} target="_blank" rel="noopener noreferrer">{user.website}</a></p>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;



Summary
Local Storage: Used to store fetched user data so that it persists between sessions.
Conditional Data Fetching: Checks local storage before making a network request.
Clear Local Storage: Added a button to clear local storage and refresh data.