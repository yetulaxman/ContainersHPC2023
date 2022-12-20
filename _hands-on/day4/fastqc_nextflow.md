---
topic: nextflow
title: Tutorial2 - FastQC example
---

## Tutorial 2: A Nextflow example with `FastQC` software
In this example,  let's use some real-world example that involves working with samples from sequencing experiments. We specifically learn:
- Declaring (and overriding default) pipeline parameters
- Moving results from analysis to a convenient folder
- Basic Nextflow channels and operators (few examples)

`fastqc_demo` folder has the necessary files for running this tutorial. You can run Nextflow script for *fastqc* analysis on your interactive terminal by issuing the following command:

```nextflow
cd fastqc_demo
nextflow run fastqc.nf 
```
### Identify resulting files from *fastqc* analysis ?

*tip*: check $PWD/work directory as shown in previous example

### Passing parameters to nextflow pipeline
Nextflow parameters inside a script are declared by prepending to a variable name with the prefix *params*, separated by dot character (e.g., params.reads). Parameters thus specified in script are used by default. The parameter can also be specified on the commandline by prefixing the parameter name with a double dash character (e.g., --reads). 
 
Here is an example to declare parameters (here, input files) to *fastqc* software inside Nextflow script (NOT for running the command on terminal):

```nextflow
params.reads = "$baseDir/data/*_{1,2}.fq.gz"
input_ch = Channel.fromFilePairs(params.reads)
```
One can also override parameter values (here files inside `$baseDir/data/` directory) in nextflow script by passing the parameters in commandline when executing script as shown below:

```bash
nextflow run fastqc.nf --reads data2/*_{1,2}_subset.fq.gz
```
Please note that *data2* folder has different samples (i.e., lymphnode4a samples) than the ones (i.e.,lung3e samples) in *data* folder which could have been used by default. You can see that *fastqc* analysis was performed on a new set of samples now as shown below:  

```
ls -l $PWD/work/*/*
```
> **_NB_**: single dash (`-`) represents core nextflow parameters (e.g., *-resume*). double dash (`--`) represents user-defined and completely extensible one -- they are used to populate `params`.

### Moving results to a convenient directory

Checking resulting files from a workflow analysis as shown above is quite tedious especially when there are several processes inside a workflow. Nextflow provides an easy way to collect resulting files to a convenient place using a special directive, *publishDir*.

Open *fastqc.nf* script in any text editor and uncomment (= remove double slashes) the following line:

```nextflow
// publishDir params.outdir 
```
and then run pipeline again. But this time, let's use *-resume* flag as we don't need to perform quality control analysis again so that actual analysis is skipped due to the capability of nextflow to track cached results from the previous analysis.  

```nextflow
nextflow run fastqc.nf -resume
```
Once the script is run successfully, you can check the files:

```
ls -l results/
````
By using `-resume` flag, the resulting files from previous analysis are simply copied to folder *results* .

### Understanding nextflow channels and operators 
Channels and operators are core features of nextflow. Please read and learn different ways of creating [Channels](https://www.nextflow.io/docs/latest/channel.html) and [operators](https://www.nextflow.io/docs/latest/operator.html) to manupulate content of channels.  

Here are few examples on how one can create channels in nextflow script:
```nextflow
Channel.create(); Channel.empty(); Channel.fromPath()
 ```
This default semantics can be changed using the channel operators that Nexflow provides, some of which are shown below:

```nextflow
split         merge         view
filter        map/reduce    group
```

What would be the output of the following nextflow script with value channels?
```bash
#!/usr/bin/env nextflow
// Examples with value channels where data is non-consumable
ch = Channel.value('Hello')
ch.view()
ch.view()
ch.view()

ch1 = Channel.value()    // empty channel
// ch1.view{ "content in ch1 channel: $it" } // if you uncomment view() method will wait for data
ch2 = Channel.value( 'Hello there' )   //  binds a string to *ch2*
ch2.view{ "content in ch2 channel: $it" } // *it* is an implicit variable
ch3 = Channel.value( [1,2,3,4,5] )  // binds a list object to *ch3* which will be emitted as a sole emission.
ch3.view{ "content in ch3 channel: $it" } 

```

<details>
<summary> <b> Click me for solution </b></summary>
<pre>	
N E X T F L O W  ~  version 20.07.1
Launching `value.nf` [elated_lumiere] - revision: b486c90fcb
Hello
Hello
Hello
content in ch2 channel: Hello there
content in ch3 channel: [1, 2, 3, 4, 5]
</pre>
</details>

What would be the output of the following nextflow script with queue channels ?
You need to be in `fastqc_demo` folder as queue channel example needs fastq files in `fastqc_demo/data`
```bash
#!/usr/bin/env nextflow
// example with queue channel where data is consumable
params.greeting  = 'Hello world!'
greeting_ch = Channel.from(params.greeting)
greeting_ch.view{ "queue channel first time: $it" }
// As this is queue channel, if you uncomment below line, you can see error: Channel `greeting_ch` has been used as an input by more than a process or an operator
// greeting_ch.view{ "queue channel second time: $it" }  

list = ['hello', 'world']
list_ch = Channel.fromList(list)
list_ch.view{ "list channel: $it" }

file_ch = Channel.fromFilePairs('./data/lung3e_subset_{1,2}.fq.gz')
file_ch.view{ "files channel: $it" }
```
<details>
<summary> <b> Click me for solution </b></summary>
<pre>	
N E X T F L O W  ~  version 20.07.1
Launching `queue.nf` [pensive_becquerel] - revision: 9ddca4d6bf
list channel: hello
list channel: world
queue channel first time: Hello world!
files channel: [lung3e_subset, [/scratch/project_2003682/yetukuri/nextflow_tutorial/fastqc_demo/data/lung3e_subset_1.fq.gz, /scratch/project_2003682/yetukuri/nextflow_tutorial/fastqc_demo/data/lung3e_subset_2.fq.gz]]
</pre>
</details>
