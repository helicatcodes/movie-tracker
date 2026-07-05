# Rails Watch List

A movie watchlist manager. Create named lists, search for movies, and bookmark them with a comment. Movie data (title, overview, poster, rating, genre, director, actors, runtime, Rotten Tomatoes score) is fetched live from the [OMDB API](https://www.omdbapi.com/) rather than stored as static fixtures.

**Status:** work in progress.

## Tech Stack

- Rails 8.1 / Ruby 3.3.5
- PostgreSQL
- Hotwire (Turbo + Stimulus), import maps — no webpack
- Bootstrap 5 + Font Awesome 6 + custom SCSS (sassc-rails)
- SimpleForm for forms
- RSpec for testing (models, controllers, requests, routing)
- dotenv-rails for environment variables
- Kamal + Docker for deployment

## Features

- Create and view watchlists
- Search for movies via OMDB and add them to a list with a comment
- Remove a movie from a list
- Delete a list (and its bookmarks)

## Models

- **Movie** — title, overview, poster_url, rating, year, genre, director, actors, runtime, rotten_tomatoes; `has_many :bookmarks`, `has_many :lists, through: :bookmarks`
- **List** — name; `has_many :bookmarks, dependent: :destroy`, `has_many :movies, through: :bookmarks`
- **Bookmark** — join model linking a list and a movie with a comment (minimum 6 characters); a movie can only appear once per list

## Routes

Routes are manually defined rather than using `resources`.

```
GET    /lists           lists#index
GET    /lists/new       lists#new
POST   /lists           lists#create
GET    /lists/:id       lists#show
DELETE /lists/:id       lists#destroy
POST   /bookmarks       bookmarks#create
DELETE /bookmarks/:id   bookmarks#destroy
GET    /movies/search   movies#search
POST   /movies/select   movies#select
```

## Setup

```bash
bundle install
echo "OMDB_API_KEY=your_key_here" > .env
bin/rails db:setup
bin/rails server
```

## Environment Variables

- `OMDB_API_KEY` — required for movie search, adding movies to a list, and seeding

## Development Commands

```bash
bin/rails server          # start dev server
bin/rails db:seed         # seed with curated OMDB-fetched lists and movies
bundle exec rspec         # run the test suite
bundle exec rubocop       # lint
bundle exec brakeman      # security scan
```
