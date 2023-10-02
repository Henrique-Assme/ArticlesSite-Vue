# Articles Website-Vue
A fullstack application using Vue.js, PostgreSQL and MongoDB
# How to run it locally
First you will have to download and install MongoDB and PostgreSQL on your computer so it can run the backend from the application. For MongoDb, run 
```
mondog
```
and wait for something like this
```
{"t":{"$date":"2023-10-01T23:36:29.661-03:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":27017,"ssl":"off"}}
```
For Postgres, run 
```
psql -U <user_name>
\c <data_base_name>
```
After that go to backend and frontend folders to install all the dependencies from both folders with
```
npm i
```
Lastly, set the .env file with authSecret and the data base info.
```
module.exports = {
    authSecret: '',
    db: {
        host: '',
        port: ,
        database: '',
        user: '',
        password: ''
    }
}
```
After that, run
```
npm start
```
at the backend folder and run
```
npm run serve
```
at the frontend folder.
The application should be running and show this login page
![image](https://github.com/Henrique-Assme/ArticlesSite-Vue/assets/69920692/ea6c52f3-4f76-4215-a3dd-541279cbb13f)

