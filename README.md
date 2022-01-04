# GAME-OFTHRONES[app.js](https://github.com/MDALENOORHUSSAIN/GAME-OFTHRONES/files/7806350/app.txt)
var ex = require('express');
var app = ex();
var m = require("mongodb");
var MClient = m.MongoClient;
var url = "mongodb://localhost:27017/";
var databasename = "THRONES";
app.set('view engine', 'ejs');
app.get("/listofPlace", function (req, res) {

    MClient.connect(url, function (err, server) {
        var db = server.db(databasename);
        
        db.collection("game").find({}).toArray(function (err, document) {
            if (err) throw err;

            res.render("listofplace", { place: document });
            console.log("document");
           


        });
    });

});
app.get("/listofbattles", function (req, res) {

    MClient.connect(url, function (err, server) {
        var db = server.db(databasename);
       
        db.collection("battle").find({}).toArray(function (err, document) {
            if (err) throw err;

            res.render("listofbattle", { battle: document });
            console.log("document");

        });
    });

});
app.get("/listofbattlesking", function (req, res) {

    MClient.connect(url, function (err, server) {
        var db = server.db(databasename);
        //var king= { king:req.query.king }
        db.collection("king").aggregate([{ $match: { name: 'Robb Stark' }}], function (err, document) {
            if (err) throw err;

            res.render("listofbattlesking", { battlesking: document });
            console.log("document");

        });
    });

});
app.get("/countbattles", function (req, res) {

    MClient.connect(url, function (err, server) {
        var db = server.db(databasename);
        
        db.collection("battle").aggregate([{ $count: "battle" }], function (err, document) {
            if (err) throw err;

            res.send("counted");
            console.log("document");

        });
    });

});

app.get("/search", function (req, res) {

    MClient.connect(url, function (err, server) {
        var db = server.db(databasename);
        var battle = { battle: req.query.battle }
        db.collection("king").aggregate([{ $match: { name: 'Robb Stark' }},
        {$match:{place:'Riverrun'}},{$match:{type:'siege'}}], function (err, document) {
            if (err) throw err;

            res.render("show", { kingsearch: document });
            console.log("document");

        });
});
       
});

app.listen(3001);
console.log("ok");
