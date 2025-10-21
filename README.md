<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CineMatch - Your Movie Companion</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            color: #fff;
            min-height: 100vh;
            overflow-x: hidden;
        }

        .navbar {
            background: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(10px);
            padding: 1.5rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logo {
            font-size: 2rem;
            font-weight: bold;
            background: linear-gradient(45deg, #ff6b6b, #ffd93d);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .nav-links {
            display: flex;
            gap: 2rem;
            list-style: none;
        }

        .nav-links a {
            color: #fff;
            text-decoration: none;
            transition: all 0.3s;
            padding: 0.5rem 1rem;
            border-radius: 8px;
        }

        .nav-links a:hover {
            background: rgba(255, 255, 255, 0.1);
            transform: translateY(-2px);
        }

        .hero {
            text-align: center;
            padding: 4rem 2rem;
            background: linear-gradient(180deg, transparent, rgba(0,0,0,0.3));
        }

        .hero h1 {
            font-size: 3.5rem;
            margin-bottom: 1rem;
            background: linear-gradient(45deg, #ff6b6b, #ffd93d, #6bcf7f);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: shimmer 3s infinite;
        }

        @keyframes shimmer {
            0%, 100% { filter: brightness(1); }
            50% { filter: brightness(1.3); }
        }

        .hero p {
            font-size: 1.3rem;
            color: #ccc;
            margin-bottom: 2rem;
        }

        .search-container {
            max-width: 600px;
            margin: 0 auto 3rem;
            position: relative;
        }

        .search-box {
            width: 100%;
            padding: 1rem 3rem 1rem 3rem;
            font-size: 1.1rem;
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 50px;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            color: #fff;
            outline: none;
            transition: all 0.3s;
        }

        .search-box:focus {
            border-color: #ff6b6b;
            box-shadow: 0 0 20px rgba(255, 107, 107, 0.3);
        }

        .search-icon {
            position: absolute;
            left: 1.2rem;
            top: 50%;
            transform: translateY(-50%);
            width: 20px;
            height: 20px;
            fill: #ccc;
        }

        .filters {
            display: flex;
            justify-content: center;
            gap: 1rem;
            flex-wrap: wrap;
            margin-bottom: 3rem;
        }

        .filter-btn {
            padding: 0.7rem 1.5rem;
            border: 2px solid rgba(255, 255, 255, 0.2);
            background: rgba(255, 255, 255, 0.05);
            color: #fff;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .filter-btn:hover, .filter-btn.active {
            background: linear-gradient(45deg, #ff6b6b, #ffd93d);
            border-color: transparent;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.3);
        }

        .movies-container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 2rem;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 2rem;
        }

        .movie-card {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            overflow: hidden;
            transition: all 0.4s;
            border: 1px solid rgba(255, 255, 255, 0.1);
            cursor: pointer;
            position: relative;
        }

        .movie-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
            border-color: #ff6b6b;
        }

        .movie-poster {
            width: 100%;
            height: 400px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 4rem;
            position: relative;
            overflow: hidden;
        }

        .movie-info {
            padding: 1.5rem;
        }

        .movie-title {
            font-size: 1.4rem;
            margin-bottom: 0.5rem;
            color: #fff;
        }

        .movie-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
            font-size: 0.9rem;
            color: #aaa;
        }

        .rating {
            display: flex;
            align-items: center;
            gap: 0.3rem;
            color: #ffd93d;
            font-weight: bold;
        }

        .movie-genre {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
            margin-bottom: 1rem;
        }

        .genre-tag {
            padding: 0.3rem 0.8rem;
            background: rgba(255, 107, 107, 0.2);
            border-radius: 15px;
            font-size: 0.8rem;
            border: 1px solid rgba(255, 107, 107, 0.3);
        }

        .movie-description {
            color: #ccc;
            font-size: 0.95rem;
            line-height: 1.5;
            margin-bottom: 1rem;
        }

        .movie-actions {
            display: flex;
            gap: 1rem;
        }

        .action-btn {
            flex: 1;
            padding: 0.7rem;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 0.95rem;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }

        .watch-btn {
            background: linear-gradient(45deg, #ff6b6b, #ff8787);
            color: #fff;
        }

        .watch-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.4);
        }

        .save-btn {
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .save-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .loading {
            text-align: center;
            padding: 3rem;
            font-size: 1.5rem;
            color: #ccc;
        }

        .icon::before {
            font-size: 1.2rem;
        }

        .icon-film::before { content: '🎬'; }
        .icon-star::before { content: '⭐'; }
        .icon-fire::before { content: '🔥'; }
        .icon-heart::before { content: '❤'; }
        .icon-comedy::before { content: '😄'; }
        .icon-action::before { content: '💥'; }
        .icon-drama::before { content: '🎭'; }
        .icon-scifi::before { content: '🚀'; }
        .icon-play::before { content: '▶'; }
        .icon-bookmark::before { content: '🔖'; }

        @media (max-width: 768px) {
            .hero h1 { font-size: 2.5rem; }
            .nav-links { display: none; }
            .movies-container {
                grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
                gap: 1.5rem;
            }
        }
    </style>
</head>
<body>
    <nav class="navbar">
        <div class="logo">
            <span class="icon icon-film"></span>
            CineMatch
        </div>
        <ul class="nav-links">
            <li><a href="#home">Home</a></li>
            <li><a href="#trending">Trending</a></li>
            <li><a href="#watchlist">Watchlist</a></li>
            <li><a href="#profile">Profile</a></li>
        </ul>
    </nav>

    <section class="hero">
        <h1>Discover Your Next Favorite Movie</h1>
        <p>Personalized recommendations just for you</p>
        
        <div class="search-container">
            <svg class="search-icon" viewBox="0 0 24 24">
                <path fill="currentColor" d="M9.5,3A6.5,6.5 0 0,1 16,9.5C16,11.11 15.41,12.59 14.44,13.73L14.71,14H15.5L20.5,19L19,20.5L14,15.5V14.71L13.73,14.44C12.59,15.41 11.11,16 9.5,16A6.5,6.5 0 0,1 3,9.5A6.5,6.5 0 0,1 9.5,3M9.5,5C7,5 5,7 5,9.5C5,12 7,14 9.5,14C12,14 14,12 14,9.5C14,7 12,5 9.5,5Z"/>
            </svg>
            <input type="text" class="search-box" id="searchBox" placeholder="Search for movies...">
        </div>

        <div class="filters">
            <button class="filter-btn active" data-genre="all">
                <span class="icon icon-fire"></span>
                All Movies
            </button>
            <button class="filter-btn" data-genre="action">
                <span class="icon icon-action"></span>
                Action
            </button>
            <button class="filter-btn" data-genre="comedy">
                <span class="icon icon-comedy"></span>
                Comedy
            </button>
            <button class="filter-btn" data-genre="drama">
                <span class="icon icon-drama"></span>
                Drama
            </button>
            <button class="filter-btn" data-genre="sci-fi">
                <span class="icon icon-scifi"></span>
                Sci-Fi
            </button>
        </div>
    </section>

    <div class="movies-container" id="moviesContainer">
        <div class="loading">Loading amazing movies...</div>
    </div>

    <script>
        const movies = [
            { title: "Quantum Paradox", year: 2024, rating: 8.7, genre: ["sci-fi", "thriller"], description: "A physicist discovers a way to manipulate time, but every change creates dangerous ripples across parallel universes.", color: "linear-gradient(135deg, #667eea 0%, #764ba2 100%)" },
            { title: "The Last Comedian", year: 2024, rating: 7.9, genre: ["comedy", "drama"], description: "In a world where laughter is forbidden, one comedian risks everything to make people smile again.", color: "linear-gradient(135deg, #f093fb 0%, #f5576c 100%)" },
            { title: "Shadow Protocol", year: 2024, rating: 8.4, genre: ["action", "thriller"], description: "An elite spy must go rogue to stop a global conspiracy that threatens world peace.", color: "linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)" },
            { title: "Echoes of Tomorrow", year: 2023, rating: 8.1, genre: ["drama", "sci-fi"], description: "A haunting tale of love and loss set in a future where memories can be bought and sold.", color: "linear-gradient(135deg, #fa709a 0%, #fee140 100%)" },
            { title: "Velocity Kings", year: 2024, rating: 7.6, genre: ["action", "adventure"], description: "Street racers compete in an underground tournament where the stakes are life or death.", color: "linear-gradient(135deg, #30cfd0 0%, #330867 100%)" },
            { title: "The Forgotten Realm", year: 2023, rating: 8.9, genre: ["fantasy", "adventure"], description: "A young warrior discovers she's the key to saving a magical kingdom from eternal darkness.", color: "linear-gradient(135deg, #a8edea 0%, #fed6e3 100%)" },
            { title: "Midnight Diner", year: 2024, rating: 8.2, genre: ["drama", "comedy"], description: "Late-night stories unfold at a small diner where strangers become family.", color: "linear-gradient(135deg, #ff9a56 0%, #ff6a88 100%)" },
            { title: "Neural Network", year: 2024, rating: 7.8, genre: ["sci-fi", "thriller"], description: "A programmer creates an AI that becomes too intelligent and begins questioning human authority.", color: "linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%)" },
            { title: "The Heist Code", year: 2023, rating: 8.3, genre: ["action", "thriller"], description: "A team of master thieves plans the ultimate heist against an impossible target.", color: "linear-gradient(135deg, #ff0844 0%, #ffb199 100%)" }
        ];

        let currentFilter = 'all';
        let searchQuery = '';

        function renderMovies() {
            const container = document.getElementById('moviesContainer');
            let filteredMovies = movies;

            if (currentFilter !== 'all') {
                filteredMovies = movies.filter(movie => movie.genre.includes(currentFilter));
            }

            if (searchQuery) {
                filteredMovies = filteredMovies.filter(movie =>
                    movie.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
                    movie.description.toLowerCase().includes(searchQuery.toLowerCase())
                );
            }

            if (filteredMovies.length === 0) {
                container.innerHTML = '<div class="loading">No movies found. Try a different search or filter!</div>';
                return;
            }

            container.innerHTML = filteredMovies.map(movie => `
                <div class="movie-card">
                    <div class="movie-poster" style="background: ${movie.color}">
                        <span class="icon icon-film" style="font-size: 4rem;"></span>
                    </div>
                    <div class="movie-info">
                        <h3 class="movie-title">${movie.title}</h3>
                        <div class="movie-meta">
                            <span>${movie.year}</span>
                            <div class="rating">
                                <span class="icon icon-star"></span>
                                ${movie.rating}
                            </div>
                        </div>
                        <div class="movie-genre">
                            ${movie.genre.map(g => `<span class="genre-tag">${g}</span>`).join('')}
                        </div>
                        <p class="movie-description">${movie.description}</p>
                        <div class="movie-actions">
                            <button class="action-btn watch-btn">
                                <span class="icon icon-play"></span>
                                Watch Now
                            </button>
                            <button class="action-btn save-btn">
                                <span class="icon icon-bookmark"></span>
                                Save
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');

            // Add button events
            document.querySelectorAll('.watch-btn').forEach((btn, index) => {
                btn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    alert(`Opening ${filteredMovies[index].title}...`);
                });
            });

            document.querySelectorAll('.save-btn').forEach((btn) => {
                btn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    btn.innerHTML = '<span class="icon icon-heart"></span> Saved!';
                    btn.style.background = 'rgba(255, 107, 107, 0.3)';
                });
            });
        }

        // Filter buttons
        document.querySelectorAll('.filter-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                currentFilter = btn.dataset.genre;
                renderMovies();
            });
        });

        // Search functionality
        const searchBox = document.getElementById('searchBox');
        searchBox.addEventListener('input', (e) => {
            searchQuery = e.target.value;
            renderMovies();
        });

        // Initial render
        setTimeout(() => {
            renderMovies();
        }, 500);
    </script>
</body>
</html>
