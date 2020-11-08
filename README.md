# Express-Mongoose-Fast-Code

## server.js

```
const express = require('express');
const mongoose = require('mongoose');
const mongo = require('./keep/mongourl');
const api = require('./routes/api');

const app = express();

/* CONNECT TO MONGOOSE */
mongoose.connect(mongo.url, {useNewUrlParser: true, useUnifiedTopology: true});

/* USE API ROUTES */
app.use('/api', api);

app.listen(9000, () => {
    console.log(`Server started on port`);
});
```


*******
## Models/Keep.js (Mongoose Model)

```
const mongoose = require('mongoose');

const KeepSchema = new mongoose.Schema({
    title: String,
    note: String
});

const KeepModel = mongoose.model('keep', KeepSchema); 

module.exports = KeepModel;
```

*****
## routes/api.js (Api Routes)

```
const express = require('express')
const router = express.Router();
const KeepModel = require('../models/Keep');

router.get('/', (req, res)=> {
    KeepModel.find({}, (err, docs) => {
        if(err){
            console.log(err);
        }else{
            res.json(docs);
        }
    });
});

module.exports = router;
```
