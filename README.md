react-spa-protected-dashboard
NAME Nithish R
REG.NO:212223040125
AIM
To develop a Single Page Application (SPA) using React.js and React Router that includes:

Public routes: Home and Login

Protected Dashboard routes: Profile, Settings, and Notifications

Proper navigation using react-router-dom

Authentication-based route protection to restrict access to dashboard pages only for logged-in users

PROCEDURE
Project Setup

Create a new React project using create-react-app

Install react-router-dom for routing

Create Components for Each Page

Home, Login, Dashboard, Profile, Settings, Notifications

Implement Authentication

Use React useState or useContext for managing login state

Create a ProtectedRoute component to restrict access to dashboard routes

Create Nested Routing for the Dashboard

Dashboard has its own internal routes: Profile, Settings, and Notifications

Test the Application

Verify that accessing protected routes redirects unauthenticated users to Login

Confirm that logged-in users can navigate dashboard subpages

PROGRAM
APP.JSX
import React, { createContext, useContext, useState } from "react";
import { BrowserRouter as Router, Routes, Route, Link, Navigate, Outlet, useNavigate } from "react-router-dom";

// Authentication Context
const AuthContext = createContext();

const AuthProvider = ({ children }) => {
  const [isAuthenticated, setAuthenticated] = useState(false);
  const login = () => setAuthenticated(true);
  const logout = () => setAuthenticated(false);

  return (
    <AuthContext.Provider value={{ isAuthenticated, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

// Protected Route Wrapper
const PrivateRoute = () => {
  const { isAuthenticated } = useContext(AuthContext);
  return isAuthenticated ? <Outlet /> : <Navigate to="/login" replace />;
};

// Pages
const Home = () => <h2>🏠 Home Page</h2>;

const Login = () => {
  const { login } = useContext(AuthContext);
  const navigate = useNavigate();
  const handleLogin = () => {
    login();
    navigate("/dashboard", { replace: true });
  };
  return (
    <div>
      <h2>🔐 Login Page</h2>
      <button onClick={handleLogin}>Login</button>
    </div>
  );
};

const Dashboard = () => {
  const { logout } = useContext(AuthContext);
  return (
    <div>
      <h2>📊 Dashboard</h2>
      <nav>
        <Link to="profile">Profile</Link> |{" "}
        <Link to="settings">Settings</Link> |{" "}
        <Link to="notifications">Notifications</Link> |{" "}
        <button onClick={logout}>Logout</button>
      </nav>
      <hr />
      <Outlet />
    </div>
  );
};

const Profile = () => <h3>👤 Profile Page</h3>;
const Settings = () => <h3>⚙️ Settings Page</h3>;
const Notifications = () => <h3>🔔 Notifications Page</h3>;

// Main App Component
export default function App() {
  return (
    <AuthProvider>
      <Router>
        <nav>
          <Link to="/">Home</Link> | <Link to="/dashboard">Dashboard</Link> |{" "}
          <Link to="/login">Login</Link>
        </nav>
        <hr />

        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/login" element={<Login />} />

          {/* Protected Dashboard */}
          <Route element={<PrivateRoute />}>
            <Route path="/dashboard" element={<Dashboard />}>
              <Route path="profile" element={<Profile />} />
              <Route path="settings" element={<Settings />} />
              <Route path="notifications" element={<Notifications />} />
            </Route>
          </Route>

          <Route path="*" element={<h2>404 Not Found</h2>} />
        </Routes>
      </Router>
    </AuthProvider>
  );
}
OUTPUT
image image image
RESULT
A functional SPA with proper navigation and authentication-based protected routes, implemented using modern React best practices and React Router v6. Users can only access dashboard content after logging in.
