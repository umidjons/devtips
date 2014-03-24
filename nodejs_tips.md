# NodeJS Tips & Tricks

#### Search a package:
`npm search <package>`

#### Detailed view of a package
`npm view <package>`

#### Install a package
`npm install <package>`

#### Install a package globally
`npm install -g <package>`
`npm install --global <package>`

#### Create `package.json` file
`npm init`

#### Install NodeJS on Linux from source
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

#### Learn NodeJS with interactive game
Install `learnyounode`:
```sh
sudo npm install -g learnyounode
```
To start game type:
```sh
learnyounode
```

#### Example `Hello World`
```js
console.log('HELLO WORLD');
```
Output:
```
HELLO WORLD
```

#### Example sum of numbers from command line
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
#### Example: Read file content synchronously and print number of lines ####
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
#### Example: Read file content asynchronously and print number of lines ####
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

#### Example: Create custom module ####
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
