# Express-Mongoose-Fast-Code

server.js

```
const express = require('express');
const mongoose = require('mongoose');
const mongoose = require('mongoose');

const app = express();
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});

const KeepSchema = new mongoose.Schema({
    title: String,
    note: String
});

const KeepModel = mongoose.model('keep', KeepSchema); 

app.get('/', (req, res)=> {
    KeepModel.find({}, (err, docs) => {
        if(err){
            console.log(err);
        }else{
            res.json(docs)
        }
    });
});

app.listen(9000, () => {
    console.log(`Server started on port`);
});
```
