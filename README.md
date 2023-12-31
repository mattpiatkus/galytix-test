# Galytix Test

This is the test for senior data engineer at Galytix.  It consists primarily of an REST API running locally on port 8000,
endpoint `/closest_phrase/` which finds the closest phrase in phrases.csv according to the word2vec embeddings given.

### Serving this app

I received an error when I attempted to pull the word2vec files programmatically and to save time I downloaded the file.

It has not been written to git but the app will expect it in the `/vectors` directory.

The app also expects `phrases.csv` in the `/data` directory.

With these files present, the app can be tested using docker compose:

`docker compose --profile test up`

The app can be run using docker compose, but there's a networking issue I've so far been unable to solve that
prevents me from getting responses using Postman on Windows, so I would need to get that working:

`docker compose --profile serve up`

The app can be successfully run without docker by installing dependencies, and this I have been able to verify:

`poetry install`

`poetry run uvicorn`

`curl --location 'http://127.0.0.1:8000/closest_phrase/'
--header 'Accept: application/json'
--header 'Content-Type: application/json'
--data '{"text": "What profits were clocked by Cholamandalam in 2020?"}'`

### Scripts

This app also contains a script `/single_use_script/process_phrases.py` which will calculate the distances between pairs of phrases and write them to /data/output.

It's not super-efficient and could be improved making more use of gensim and numpy instead of native python.

### To do

- Error handling - for example, return sensible messages when the app is run without the input files.  FastAPI does quite a bit of http error handling but maybe not enough.
- Logging:  there is none.
- Refactor processor to be more efficient
- More in-line comments?  Are there enough?  Review.
- Get vectors from database?  Phrases from database?  What should go in-memory?
- Tokenisation?  There is none