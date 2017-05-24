Are We Compressed Yet?
====
This repository contains the arewecompressedyet.com website source code.

Running your own local copy of the website
===
To run a local copy, you will need a copy of node and npm.

First, run the `./setup.sh` script. It will create directories needed for AWCY to run.

Next, create a configuration file called `config.json`.

Here is an example config.json:

```
{ "channel": "#daalatest",
  "have_aws": false,
  "port": 3000,
  "rd_server_url": "http://localhost:4000"
}
```

You will also need a file called `secret_key` which contains the key needed to use the website.

```
echo 'fake_password_to_compile' > secret_key
```

These commands will create the configuration files and install the node.js modules that get used by
awcy. Open a node command line and run the following:

```
npm install
npm start
```

To run the server, execute the run_awcy.bat file
or run the following in your command line:
```
  node awcy_server.js
```
Now you can open localhost:3000 with your browser to see your local version of the website.

Setting up repositories
===
For the website to build codecs, you need local checkouts of every codec. While the `./setup.sh` script
automatically adds AV1, you can add your own. For example:
```
git clone https://aomedia.googlesource.com/aom av1
ln -s av1 av1-rt
```

Setting up rd_server
===
The AWCY web server manages the repositories and runs/ directory, and compiles and builds the codecs. Once this is done, it hands off the job of actually talking to all the AWS machines to rd_server.

To install rd_server, checkout the rd_tool repository in the same directory as awcy:
```
git clone https://github.com/tdaede/rd_tool.git
```

Then start the rd_server.py daemon:
```
./rd_server.py
```
The rd_server.py daemon listens on port 4000 by default.

More documentation on rd_server.py can be found in its README.

Run database format
===
The runs/ directory will contain all of the output files generated from a job. There is a info.json file that specifies what options were used by that particular run. Here is an example of an info.json file:

  {"codec":"daala","commit":"","run_id":"2014-09-19T22-00-08.196Z","task":"video-1-short","nick":"AWCY","task_type":"video"}

There is also an output.txt file that contains the output of the rd_tool.py script.

After each run, a cache file called list.json file is generated by the generate_list.js script. This contains all of the info.json files, as an ordered list. This should probably be replaced by a "real" database at some point.
