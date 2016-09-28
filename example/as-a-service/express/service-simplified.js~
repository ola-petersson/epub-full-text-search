const express = require('express'),
	searchEngine = require('../../../');
	var searchText; 


var SampleService = function () {
 	var self = this;
    	self.app = express();
 
	function setupVariables() {

	//	self.ipaddress = process.env.IP;
		self.port = process.env.PORT || 8080;

		//if (typeof self.ipaddress === "undefined")
		    self.ipaddress = "127.0.0.1";
	    }

	function createRoutes() {
		self.routes = {};

		self.routes['/search'] = function (req, res) {

		    if (!req.query['q']) {
		        res.status(500).send('Can`t found query parameter q -> /search?q=word');
		        return;
		    }
			
			
			//console.log(req.query['q'].toLowerCase());
			searchText = req.query['q'].toLowerCase();
		    //console.log( req.query['q']) 
		    console.log('searchText = ' + searchText);
		    
		    var q = req.query['q'].toLowerCase().replace(/å/g, ' ').replace(/ä/g, ' ').replace(/ö/g, ' ').replace(/\./g,' ').replace(/\,/g,' ').replace(/\-/g,' ').split(/\s+/);
		    
		    console.log('q = ' + q);
		    
		    
		    var bookTitle = req.query['t'];
			console.log('booktitle = ' +bookTitle);
		    bookTitle = bookTitle || '*'; // if bookTitle undefined return all hits
			//console.log(bookTitle)
		    searchEngine({}, function (err, se) {

		        if (err)
		            return console.log(err);

		        se.search(searchText, q, bookTitle, function (result) {

		            res.send(result);
		            se.close(function (err) {
		                if (err)
		                    console.log(err);
		            });
		        });
		    });
		};
	}

	function initServer() {

		createRoutes();
		for (var r in self.routes) {
            		self.app.get(r, self.routes[r]);
        	}
	    }

	function terminator(sig) {
		if (typeof sig === "string") {
		    console.log('%s: Received %s - terminating service ...',
		        Date(Date.now()), sig);
		    process.exit(1);
		}
		console.log('%s: Node server stopped.', Date(Date.now()));
	    }


	function setupTerminationHandlers() {
		//  Process on exit and signals.
		process.on('exit', function () {
		    terminator();
		});

		['SIGHUP', 'SIGINT', 'SIGQUIT', 'SIGILL', 'SIGTRAP', 'SIGABRT',
		    'SIGBUS', 'SIGFPE', 'SIGUSR1', 'SIGSEGV', 'SIGUSR2', 'SIGTERM'
		].forEach(function (element, index, array) {
		        process.on(element, function () {
		            terminator(element);
		        });
		    });
	    }

    	self.startIndexing = function () {

        	//var node_modules_path = require.resolve('body-parser').split('body-parser')[0]; // absolute path 
        	//var epubs = node_modules_path + 'epub3-samples';
       		var epubs = '/var/www/instudy/content/books';

        	if (process.env.DEBUG) {
            		console.log("debug mode");
            		epubs = '/var/www/instudy/content/books';
        		}

        	searchEngine({}, function (err, se) {

          	 	if (err) {
           	     	console.log(err);
           	 	} else {

           	    	se.indexing(epubs, function (info) {
            	        console.log(info);

            	        se.close(function (err) {
                       	if (err)
         	       	console.log(err);
                    	});
                	});
            		}
        	});
    	};
	
	self.init = function () {
        	setupVariables();
		setupTerminationHandlers();
		initServer();
    	};
	self.start = function () {
		//Start the app on the specific interface (and port).
		self.app.listen(self.port, self.ipaddress, function () {
		    console.log('%s: Node server started on %s:%d ...',
		      Date(Date.now()), self.ipaddress, self.port);
		});
	    };
	

};


var sase = new SampleService();
sase.startIndexing();
sase.init();
sase.start();





