---
title: "OCLint integration in XCode with xctool"
excerpt: "A detailed overview of OCLint as static analyzer with XCode"
tags: 
  - oclint
  - static analyzer
  - xcode
modified: 2016-03-28
comments: false
categories:
  - Post
---
{% include toc %}

In a hurry to do things fast and meet monster deadlines, developers keep the book of best practices aside and start coding. The result can lead to unexpected behavior in your project along with highly unmanaged code. Start following best practices of coding, with time it will become a habit.

Even the experienced programmers sometimes can't foresee the issues embedded in their code. 

OCLint comes with a huge list of options which are way too lengthy anc can't be covered in a single reading. In this tutorial we will cover best configurations suggested by OCLint documentation for XCode project.

# About OCLint
<!--OCLInt stands for ??, developed by person company.-->

[OCLint](http://oclint.org/) is a static analyzer for C, C++ and objective c. The [documentation](http://docs.oclint.org/en/stable/) to kick off your relationship with OCLint is huge and you will be jumping from one page to another in pursuit of something, something digestible. In this tutorial our goal is to Install, Integrate & View Analysis report of OCLint in XCode objective-c project. 

# Prerequisite
- [Homebrew](http://brew.sh/)
- [xctool](https://github.com/facebook/xctool)
- XCode
- OSX

# Installation

#### 1. Homebrew, Easy and Recommended

If [homebrew](http://brew.sh/) is configured on your system, you can use [homebrew tap](https://github.com/oclint/homebrew-formulae) for oclint.

{% highlight text %}
$ brew tap oclint/formulae
$ brew install oclint
{% endhighlight %}

And can be simplified to

{% highlight text %}
$ brew install oclint/formulae/oclint
{% endhighlight %}

Use following commands to update to latest version

{% highlight text %}
$ brew update
$ brew upgrade oclint
{% endhighlight %}


#### 2. Download
Ignore it if you have completed installation via Homebrew. 

Go to [OCLint Releases](https://github.com/oclint/oclint/releases) on github and download latest oclint-{versionno-architecture-OS}.zip/.tar.zip. Extract the files.

{% highlight text %}
-oclint-x.y.z
  |--LICENSE
  |--bin (Contains OCLint commands, from here you can execute it)
  |--lib (Contains clang static analyzer, reporters and rules library)
{% endhighlight %}

#### Set Path
Once installation is complete you can set path in terminal

{% highlight text %}
OCLINT_HOME=/path/to/oclint-x.y.z
export PATH=$OCLINT_HOME/bin:$PATH
{% endhighlight %}

#### Verify Installation
Check whether installation is successful. If you see the output listed below, congratulations you did it.

{% highlight text %}
$ oclint
oclint: Not enough positional command line arguments specified!
Must specify at least 1 positional arguments: See: oclint -help
{% endhighlight %}


# OCLint Commands 

#### 1. oclint
OCLint comes with rich set of options which you can use with 'oclint' command. In terminal type `$ ocline --help` to get detailed list of configurations.

#### 2. oclint-json-compilation-database
It is great that OCLint provides us options to specify each file's configurations. But in practical life our projects contains hundreds of files and it will give you a headache to do this manually. Here comes the solution `oclint-json-compilation-database`

# Integration with XCode

<figure>
<figcaption>1. Create a new aggregate Target</figcaption>
<a href="/images/OCLintInXCodeAndXCTool/XCodeNewAggregateTarget.png"><img src="/images/OCLintInXCodeAndXCTool/XCodeNewAggregateTarget.png"></a>
</figure>

<figure>
<figcaption>Name the target OCLint.</figcaption>
<a href="/images/OCLintInXCodeAndXCTool/XCodeNewAggregateTarget2.png"><img src="/images/OCLintInXCodeAndXCTool/XCodeNewAggregateTarget2.png"></a>
</figure>

<figure>
<figcaption>Add new runscript.</figcaption>
<a href="/images/OCLintInXCodeAndXCTool/AddNewRunScript.png"><img src="/images/OCLintInXCodeAndXCTool/AddNewRunScript.png"></a>
</figure>

Add the following script to new run script.

~~~ shell
echo "Building ${TARGET_NAME}"
maxPriority=15000
#Add OCLint bin directory to PATH
export PATH=/usr/local/Cellar/oclint/0.10.2/bin:$PATH
#Set Build path
BUILD_PATH=${TARGET_TEMP_DIR}
#Check whether OCLint exists or not
hash oclint &> /dev/null
if [ $? -eq 1 ]; then
echo "OCLint not found, analyzing stopped"
exit 1
else
echo "OCLint is installed"
fi
#Navigate to build path directory
cd ${BUILD_PATH}

if [ ! -f compile_commands.json ]; then
echo "compile_commands.json not found, possibly clean was performed";
else
echo "Removing compile_commands.json"
rm compile_commands.json
fi
# clean previous output
if [ -f xcodebuild.log ]; then
echo "Removing xcodebuild.log"
rm xcodebuild.log
fi

cd ${SRCROOT}

#Clean, Build and generate compile_commands.json project
xctool -project DemoOCLintTargetInProject.xcodeproj \
-configuration Debug -scheme DemoOCLintTargetInProject \
-sdk iphonesimulator  \
-reporter json-compilation-database:${BUILD_PATH}/compile_commands.json clean build

echo "Starting analyzing"
cd ${BUILD_PATH}
oclint-json-compilation-database -v -e Pods oclint_args \
"-max-priority-1=$maxPriority -max-priority-2=$maxPriority \
-max-priority-3=$maxPriority -rc LONG_LINE=500 \
-rc LONG_VARIABLE_NAME=100  -disable-rule=UnusedMethodParameter" \
| sed 's/\(.*\.\m\{1,2\}:[0-9]*:[0-9]*:\)/\1 warning:/'

~~~

# Sample project
[DemoOCLintTargetInProject](https://github.com/nabeelarif/DemoOCLintTargetInProject) is the project we used for above demo and is published on git.

<!--#Where to go from here?-->
<!---->
<!--#Wanna Master OCLint?-->
<!---->
<!--#TODO-->
<!--- http://codingfingers.com/index.html%3Fp=11065.html-->
<!--- https://gavrix.wordpress.com/2013/02/28/integrating-oclint-in-xcode/-->
