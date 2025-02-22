# Aicte
#Signin modifications
import React, { useState } from 'react';
import axios from 'axios';
import { useNavigate } from 'react-router-dom';

function Signin() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const [loading, setLoading] = useState(false); // Added loading state
  const navigate = useNavigate();

  const handleSignin = async (e) => {
    e.preventDefault();
    setError('');
    setLoading(true); // Set loading to true when request starts

    try {
      const res = await axios.post('http://localhost:5001/signin', { username, password });
      console.log('Signin Response:', res.data);

      if (res.data.token) {
        localStorage.setItem('authToken', res.data.token);
        navigate('/dashboard'); // Redirect to dashboard
      } else {
        setError('Invalid credentials');
      }
    } catch (err) {
      console.error('Signin Request Error:', err.response?.data || err.message);
      setError(err.response?.data?.message || 'Something went wrong. Please try again.');
    } finally {
      setLoading(false); // Reset loading state
    }
  };

  return (
    <div className="form-container">
      <h2>Signin</h2>
      {error && <p className="error">{error}</p>}
      <form onSubmit={handleSignin}>
        <input
          type="text"
          placeholder="Username"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
          required
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          required
        />
        <button type="submit" disabled={loading}>
          {loading ? 'Signing in...' : 'Signin'} {/* Button shows loading state */}
        </button>
      </form>
      <p className="redirect">
        Don't have an account? <a href="/signup">Signup here</a> {/* Added signup link */}
      </p>
    </div>
  );
}

export default Signin;


###signup Changes 

import React, { useState } from 'react';
import axios from 'axios';
import { useNavigate } from 'react-router-dom';

function Signup() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [confirmPassword, setConfirmPassword] = useState(''); // Added confirm password field
  const [error, setError] = useState('');
  const [loading, setLoading] = useState(false); // Added loading state
  const navigate = useNavigate();

  const handleSignup = async (e) => {
    e.preventDefault();
    setError('');

    if (!username || !password || !confirmPassword) {
      setError('All fields are required');
      return;
    }

    if (password !== confirmPassword) {
      setError('Passwords do not match');
      return;
    }

    setLoading(true); // Set loading state
    try {
      const res = await axios.post('http://localhost:5001/signup', {
        username,
        password,
      });

      alert('Signup successful! Please sign in.');
      navigate('/signin'); // Redirect to signin page
    } catch (err) {
      console.error('Signup Error:', err.response?.data || err.message);
      setError(err.response?.data?.message || 'Signup failed. Please try again.');
    } finally {
      setLoading(false); // Reset loading state
    }
  };

  return (
    <div className="form-container">
      <h2>Signup</h2>
      {error && <p className="error">{error}</p>}
      <form onSubmit={

