# Express-Mongoose-Fast-Code

- client
 - node_modules
 - public
 - src 
  - action 
   - action.js
  - components
  - Reducer
   - noteReducer.js
   - rootReducer.js
  - index.js
  - store.js
- models
 - Keep.js
- routes
 - api.js
- server.js

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

router.post('/', (req, res) => {
    let title = req.body.title;
    let note = req.body.note;
    const newdata = new KeepModel({
        title: title,
        note: note
    })
    newdata.save()
    .then(data => res.send(data));
})

router.post('/del/', (req, res)=> {
    KeepModel.deleteOne({_id: req.body.id}, (err) => {
        if(err){
            console.log(err)
        }
    })
});

module.exports = router;
```

## client/store.js (redux store)
```
import {createStore, applyMiddleware} from 'redux';
import reducers from './Reducer/rootReducer';
import thunk from 'redux-thunk';

const store = createStore(reducers, applyMiddleware(thunk));

export default store;
```

## client/reducers/rootreducer.js (combine reducer)
```
import {combineReducers} from 'redux';
import noteReducer from './noteReducer';
  
const reducers = combineReducers({
  noteReducer
});

export default reducers;
```

## client/reducers/noteReducer.js (store reducer function)
```
const initialState = [];

function noteReducer(state = initialState, action){
    switch(action.type){
      case "GETNote": 
        state = [...action.data]
      ;break;
      case "ADDNote":  
        state = [...state, action.data]
      ; break;   
      case "DELNote":  
        state = state.filter((d) => {
            console.log(action.delId)
            return action.delId !== d._id;
        })
      ; break;
      default : return state;
    }
    return state;
  }

export default noteReducer;
```

## client/action.js (give action to store)
```
import axios from 'axios';

export const getNotedata = () => dispatch => {
    axios.get('/api/')
    .then(res => res.data)
    .then(data => dispatch({type: 'GETNote', data}))
}

export const addNotedata = (title, note) => dispatch => {
    axios.post('/api/', {title, note})
    .then(res => res.data)
    .then(data => dispatch({type: 'ADDNote', data}))
}

export const delNotedata = id => {
    axios.post('/api/del/', {id})
    return {
        type: 'DELNote',
        delId : id
    }
}
```
