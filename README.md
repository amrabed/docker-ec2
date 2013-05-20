<h1>Docker-EC2</h1>

<p>This script automates the process of running and testing an application using Docker image running on different Amazon AWS EC2 instances, provided by the user</p> 

<h2>Quick Start</h2>
<ol>
<li>Download the files from <a href="https://github.com/amrabed/docker-ec2.git">here </a> </li>

<li>Update the fields of the Vagrantfile in the vagrant/ folder to match your own AWS credentials</li>

<li>Run the script: <code>./docker-ec2 -h</code></li>
</ol>

<h2>How It Works</h2>
<p>The script:
<ol>
<li> Reads needed information provided in the input file, the VagrantFile, and the command line options</li>
<li>Connects to the list of instance types provided</li>
<li>Installs Docker on each instance</li>
<li>Starts the application by running the provided command on the provided Docker app image</li>
<li>Tests the application by running the provided test command on the provided Docker test image</li>
<li>Copies test results back to local machine (requires SSH port enabled on test image)</li>
</ol>

<h2>Usage</h2>
<pre><code>
Usage: ./docker-ec2 input_file [-k] [-i instance1[:instance2[:...]]]
or
       ./docker-ec2 -h   (for help)

  -i    - List of EC2 instance types (default: m1.small)
  -k    - Keep instances from previous runs (default: false)
  -h    - Print this message
</code></pre>

<h3>Input File<h3>
<p><b>PLEASE NOTE THAT running Docker-EC2 using the sample input file may result in costs on your Amazon AWS account. Please read and modify the file as needed to match your preferences before running the script</b></p>
<p>The input file 'sample.in' provides an example input file showing the format and the full list of supported specs for application and test</p>
<p><code>app.image</code> (required): The Docker image for the application under test</p>
<p><code>app.command</code> (required): The command to be run on the Docker image to start the application</p>
<p><code>app.port</code> : The application port (e.g. 80 for HTTP server)</p>
<p><code>app.instances</code> :  The list of EC2 instance types (separated by commas ',' or colons ':' )</p>

<p><code>test.image</code> :  The Docker image for the test</p>
<p><code>test.command</code> : The command to be run on the Docker image to start the test</p>
<p><code>test.port</code> : The port at which the test results are accessible (e.g. 22 for SSH/SCP)</p>

<h3>Instance Types</h3>
<p>Instance types can be specified either by using the input file using the <code>app.instances</code> specifier, or using the <code>-i</code> commend line option.</p>
<p>If both are specified, the command line option overrides the input file specification.</p>
<p>If none is specified, a default 'm1.small' is assumed.</p>

<h2>Clean Up</h2>
<p>By default, each run cleans all traces of previous runs, i.e. deletes created workspace folder (including output files), and terminates any running EC2 instances</p>
<p>To disable the auto-cleanup feature of the script, use the <code>-k</code> option. Using this option, output files created from subsequent runs may override files from previous runs (if they share the same instance types). However, running EC2 instances will be re-used.</p>
<p>To manually clean traces from previous runs, and/or terminate running EC2 instances, use the <code>clean</code> script as follows:
<pre><code>
Usage: ./clean [-n] [all|log|output|instance]
or
       ./clean -h   (for help)

  all   	- Delete all generated files and folders  
  log		- Delete log files only
  output	- Delete output folders only
  instance	- Delete instance folders only (default)

  -n    	- Do NOT terminate EC2 instance(s)
  -h		- Display this message
</code></pre>

<h2>To Do</h2>
<ul>
<li>Fix problem where app/test terminates (does not run as a service)</li>
<li>Automate plotting of received results</li>
<li>Allow test to be run on a separate instance type</li>
<li>Handle hashing in input file</li>
<li>Add Code Documentation</li>
<li>Update this file</li>
</ul>

