# ## Social Media API Backend (FastAPI + Postgres)

This is a Dockerized FastAPI backend for a simple social media app (users, posts, likes/votes) backed by PostgreSQL and SQLAlchemy.  
It includes JWT-based authentication, Alembic migrations, and a small test suite using pytest.

#### This API  has 4 routes

## 1) Post route

#### This route is reponsible for creating post, deleting post, updating post and Checkinh post

## 2) Users route

#### This route is about creating users and searching user by id

## 3) Auth route

#### This route is about login system

## 4) Vote route

 #### This route is about likes or vote system and this route contain code for upvote or back vote there is not logic about down vote

# how to run locally
First clone this repo by using following command
````

git clone https://github.com/Sanjeev-Thiyagarajan/fastapi-course.git

````
then 
````

cd fastapi-course

````

Then install fastapp using all flag like 

````

pip install fastapi[all]

````

Then go this repo folder in your local computer run follwoing command
````

uvicorn main:app --reload

````

Then you can use following link to use the  API

````

http://127.0.0.1:8000/docs 

````

## After run this API you need a database in postgres 
Create a database in postgres then create a file name .env and write the following things in you file 

````
DATABASE_HOSTNAME = localhost
DATABASE_PORT = 5432
DATABASE_PASSWORD = passward_that_you_set
DATABASE_NAME = name_of_database
DATABASE_USERNAME = User_name
SECRET_KEY = 09d25e094faa2556c818166b7a99f6f0f4c3b88e8d3e7 
ALGORITHM = HS256
ACCESS_TOKEN_EXPIRE_MINUTES = 60(base)

````
### Note: SECRET_KEY in this exmple is just a psudo key. You need to get a key for youself and you can get the SECRET_KEY  from fastapi documantion
 


### How this API is structured

- **Tech stack**: FastAPI, SQLAlchemy ORM, PostgreSQL, Alembic (migrations), Docker, pytest.
- **App entrypoint**: `app/main.py` creates the `FastAPI` app, registers routers, and configures CORS.
- **Routers**:
  - `app/routers/user.py`: user registration + lookup
  - `app/routers/auth.py`: username/password login → JWT access token
  - `app/routers/post.py`: CRUD for posts + listing with vote counts
  - `app/routers/vote.py`: like/unlike (upvote) logic
- **Persistence layer**:
  - `app/database.py`: builds the SQLAlchemy engine and session factory from env vars
  - `app/models.py`: SQLAlchemy models for `users`, `posts`, `votes`
  - `alembic/`: database migrations that create/update the schema
- **Security**:
  - `app/oauth2.py`: issues and validates JWTs
  - `app/utils.py`: password hashing and verification (bcrypt)
- **Tests**:
  - `tests/`: pytest-based integration tests that spin up a separate test database.


### Request flow example: creating a post

1. **Client** sends `POST /posts/` with a JSON body and a `Bearer` token.
2. **Routing**: `app/main.py` forwards the request to `app/routers/post.py` because the path matches the `/posts` router.
3. **Dependencies**:
   - FastAPI injects a DB session from `app.database.get_db`.
   - FastAPI injects the current user from `app.oauth2.get_current_user` by decoding the JWT.
4. **Business logic**:
   - The request body is validated against `PostCreate` in `app.schemas`.
   - A `Post` SQLAlchemy model is created and persisted via the injected `Session`.
5. **Response**:
   - The saved `Post` model is serialized back to JSON using the `Post` schema (with `orm_mode=True`), and returned with HTTP 201.



### Customizations in this fork

This fork includes a few tweaks on top of the original course project:

- **Updated documentation**: clearer description of the architecture, request flow, and how the API is wired together.
- **Docker-first setup**: verified Docker Compose workflow with Postgres 16 and Alembic migrations.
- **Production-ready config notes**: recommendations around using `.env` + `env_file` for secrets instead of hardcoding values.

More changes will be added over time as I experiment with FastAPI patterns and features.