# Tutorial: Building a TMDB Movie App with React

This tutorial will guide you through creating a React application that fetches movie data from the TMDB API and displays it in the frontend. We'll cover setting up the project, creating components, fetching data, and basic styling.

## Prerequisites

- Basic knowledge of React and JavaScript
- Node.js and npm installed on your machine
- A TMDB API key (sign up at https://www.themoviedb.org/signup)

## Step 1: Set up the project

1. Create a new React project using Create React App:
   ```
   npx create-react-app tmdb-movie-app
   cd tmdb-movie-app
   ```

2. Install necessary dependencies:
   ```
   npm install axios react-router-dom
   ```

3. Create a `.env` file in the root of your project:
   ```
   touch .env
   ```

4. Open the `.env` file and add your TMDB API key:
   ```
   REACT_APP_TMDB_API_KEY=your_actual_api_key_here
   ```
   Note: In Create React App, environment variable names must start with `REACT_APP_` to be accessible in your JavaScript code.

5. Add `.env` to your `.gitignore` file to prevent it from being committed to version control:
   ```
   echo ".env" >> .gitignore
   ```

## Step 2: Create the basic app structure

1. Replace the contents of `src/App.js` with:

```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Header from './components/Header';
import Sidebar from './components/Sidebar';
import MovieList from './components/MovieList';
import './App.css';

function App() {
  return (
    <Router>
      <div className="App">
        <Header />
        <div className="main-content">
          <Sidebar />
          <Routes>
            <Route path="/" element={<MovieList />} />
          </Routes>
        </div>
      </div>
    </Router>
  );
}

export default App;
```

## Step 3: Create components

Create a `components` folder in the `src` directory:
```
mkdir src/components
```

1. Create `src/components/Header.js`:

```jsx
import React from 'react';

function Header() {
  return (
    <header>
      <h1>TMDB Movie App</h1>
    </header>
  );
}

export default Header;
```

2. Create `src/components/Sidebar.js`:

```jsx
import React from 'react';
import { Link } from 'react-router-dom';

function Sidebar() {
  return (
    <nav className="sidebar">
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/popular">Popular Movies</Link></li>
        <li><Link to="/top-rated">Top Rated Movies</Link></li>
      </ul>
    </nav>
  );
}

export default Sidebar;
```

3. Create `src/components/MovieList.js`:

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const API_KEY = process.env.REACT_APP_TMDB_API_KEY;
const API_BASE_URL = 'https://api.themoviedb.org/3';

function MovieList() {
  const [movies, setMovies] = useState([]);

  useEffect(() => {
    const fetchMovies = async () => {
      try {
        const response = await axios.get(`${API_BASE_URL}/movie/popular?api_key=${API_KEY}`);
        setMovies(response.data.results);
      } catch (error) {
        console.error('Error fetching movies:', error);
      }
    };

    fetchMovies();
  }, []);

  return (
    <div className="movie-list">
      <h2>Popular Movies</h2>
      <div className="movie-grid">
        {movies.map((movie) => (
          <div key={movie.id} className="movie-card">
            <img 
              src={`https://image.tmdb.org/t/p/w500${movie.poster_path}`} 
              alt={movie.title} 
            />
            <h3>{movie.title}</h3>
            <p>Release Date: {movie.release_date}</p>
            <p>Rating: {movie.vote_average}/10</p>
          </div>
        ))}
      </div>
    </div>
  );
}

export default MovieList;
```

## Step 4: Add styling

Replace the contents of `src/App.css` with:

```css
.App {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

header {
  background-color: #282c34;
  padding: 20px;
  color: white;
}

.main-content {
  display: flex;
  flex: 1;
}

.sidebar {
  width: 200px;
  background-color: #f0f0f0;
  padding: 20px;
}

.movie-list {
  flex: 1;
  padding: 20px;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.movie-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease-in-out;
}

.movie-card:hover {
  transform: scale(1.05);
}

.movie-card img {
  width: 100%;
  height: auto;
}

.movie-card h3 {
  margin: 10px;
  font-size: 1em;
}

.movie-card p {
  margin: 5px 10px;
  font-size: 0.8em;
  color: #666;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
}
```

## Step 5: Update index.js

Replace the contents of `src/index.js` with:

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

reportWebVitals();
```

## Step 6: Run the application

1. Ensure that you've added your actual TMDB API key to the `.env` file.

2. Start the development server:
   ```
   npm start
   ```

3. Open your browser and navigate to `http://localhost:3000` to see the application.

## Conclusion

You now have a functional React application that fetches movie data from the TMDB API and displays it in a visually appealing grid. The app includes a header, sidebar for navigation, and a main content area displaying popular movies.

## Next steps

To further enhance your application, consider:

1. Implementing the "Popular" and "Top Rated" pages
2. Adding a search functionality
3. Creating a detailed view for individual movies
4. Implementing pagination or infinite scrolling for the movie list
5. Adding error handling and loading states

Remember to always keep your API key secure and never commit it to version control. When deploying your application, ensure that you set up the environment variables on your hosting platform.
