# NodeJS Tips & Tricks
- [Search a package](#search-a-package)
- [Detailed view of a package](#detailed-view-of-a-package)
- [Install a package](#install-a-package)
- [Install a package globally](#install-a-package-globally)
- [Create `package.json` file](#create-packagejson-file)
- [Install NodeJS on Linux from source](#install-nodejs-on-linux-from-source)
- [Learn NodeJS as interactive game](#learn-nodejs-as-interactive-game)
- [Sum of numbers from command line](#sum-of-numbers-from-command-line)
- [Read file content synchronously](#read-file-content-synchronously)
- [Read file content asynchronously](#read-file-content-asynchronously)
- [Create custom module](#create-custom-module)
- [Performing HTTP GET request](#performing-http-get-request)

# Search a package
`npm search <package>`

# Detailed view of a package
`npm view <package>`

# Install a package
`npm install <package>`

# Install a package globally
`npm install -g <package>`
`npm install --global <package>`

# Create `package.json` file
`npm init`

# Install NodeJS on Linux from source
Download source from site:
```sh
wget http://nodejs.org/dist/v0.10.26/node-v0.10.26.tar.gz
```
Extract archive:
```sh
tar -zxfv node-v0.10.26.tar.gz
```
Change directory:
```sh
cd node-v0.10.26
```
Install `g++`:
```sh
sudo apt-get install g++
```
Compile and install:
```sh
./configure
make
sudo make install
```
Check `node` version with:
```sh
node -v
```

# Learn NodeJS as interactive game
Install `learnyounode`:
```sh
sudo npm install -g learnyounode
```
To start game type:
```sh
learnyounode
```

# Sum of numbers from command line
We use `process.argv` to accept arguments from command line:
```js
console.log(process.argv);
```
Run this script with arguments supplied:
```sh
node baby.js 1 2 3
```
Output:
```
[ 'node', '/home/monster/mynode/1/baby.js', '1', '2', '3' ]
```
Actual arguments begins with 3 position (2 index). So our sum script will look like this:
```js
var sum=0;
for(i=2; i<process.argv.length; i++)
	sum += +process.argv[i]; // + after = converts string into number
console.log('Sum:', sum);
```
Output:
```
Sum: 6
```
# Read file content synchronously
```js
if(process.argv.length<3)
{
        console.error('File not supplied.');
        process.exit(1);
}

var linesCount=require('fs')
	.readFileSync(process.argv[2])
	.toString()
	.split('\n')
	.length-1;
console.log(linesCount);
```
# Read file content asynchronously
```js
if(process.argv.length<3)
{
	console.error('File not supplied.');
	process.exit(1);
}

require('fs').readFile(process.argv[2], function(err, data)
{
	if(err)
		console.error(err);
	else
		console.log(data.toString().split('\n').length-1);
});
```

If we supply `utf-8` as second parameter to `readFile`, then no `data.toString()` call needed.

```js
if(process.argv.length<3)
{
	console.error('File not supplied.');
	process.exit(1);
}

require('fs').readFile(process.argv[2], 'utf-8', function(err, data)
{
	if(err)
		console.error(err);
	else
		console.log(data.split('\n').length-1);
});
```

# Create custom module
Read files from specified directory and filter by given extension.
File `myfilter.js`:
```js
module.exports.filter=function(_dir, _ext, _cb)
{
        var fs=require('fs');
        var path=require('path');
        var filtered_files=[];
        fs.readdir(_dir, function(err,files)
        {
                if(err)
                        return _cb(err,filtered_files);
                files.forEach(function(f,idx,arr)
                {
                        if(path.extname(f)=='.'+_ext)
                                filtered_files.push(f);
                });
                return _cb(null, filtered_files);
        });
}
```
File `my.js`:
```js
var f=require('./myfilter'); // include custom module
f.filter(process.argv[2], process.argv[3], function(err,files)
{
        if(err)
                console.error(err);
        else
                console.log(files); // output filtered files
});
```
Executing program:
```sh
node my.js /path/to/files txt
```
## Further improvements to filter items in array

Use `Array.filter` to filter items:

```js
module.exports = function( dir, ext, cb )
{
	require( 'fs' ).readdir( dir, function( err, data )
	{
		if ( err )
			return cb( err );
		var path = require( 'path' );
		var files = data.filter( function( f )
		{
			return path.extname( f ) == '.' + ext;
		} )
		return cb( null, files );
	} );
};
```

# Performing HTTP GET request
Perform HTTP GET request to the URL from command-line and print it out into the console.
```js
require( 'http' ).get( process.argv[2], function( response )
{
	// type of response is Stream
	// convert Buffer into string setting encoding
	response.setEncoding( 'utf8' );
	response.on( 'data', console.log ); // on 'data' print it into the stdout
	response.on( 'error', console.error ); // on 'error' print it into the sdterr
} );
```
