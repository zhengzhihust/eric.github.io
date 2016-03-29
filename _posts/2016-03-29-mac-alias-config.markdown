---
layout: post
title:  "Mac 命令别名设置"
date:   2016-03-28 22:39:00 +0800
categories: jekyll update
group: navigation
author: Eric Zheng
tags: efficiency
---


Mac 命令别名设置

# Path to your oh-my-zsh installation.
export ZSH=$HOME/.oh-my-zsh

# Set name of the theme to load.
# Look in ~/.oh-my-zsh/themes/
# Optionally, if you set this to "random", it'll load a random theme each
# time that oh-my-zsh is loaded.
ZSH_THEME="random"

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion. Case
# sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment the following line to disable bi-weekly auto-update checks.
# DISABLE_AUTO_UPDATE="true"

# Uncomment the following line to change how often to auto-update (in days).
# export UPDATE_ZSH_DAYS=13

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# The optional three formats: "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git ant github go golang)

# User configuration

export PATH=$HOME/bin:/usr/local/bin:$PATH
# export MANPATH="/usr/local/man:$MANPATH"
export JAVA_6_HOME=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
export JAVA_7_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home
export JAVA_HOME=$JAVA_7_HOME
export CATALINA_HOME=/Library/Tomcat

# java home
export JAVA_HOME="$(/usr/libexec/java_home)"

# hadoop path
export HADOOP_HOME=/Users/yuanlang/hadoop/hadoop-2.6.2
export PATH=$HADOOP_HOME/bin:$PATH
#export CLASSPATH=$HADOOP_HOME/lib/native:$CLASSPATH
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"

# hive path
export HIVE_HOME=/usr/local/Cellar/hive/1.1.0/libexec
export HCAT_HOME=/usr/local/Cellar/hive/1.1.0/libexec/hcatalog
export HIVE_INSTALL=/usr/local/Cellar/hive/1.1.0
export PATH=$PATH:$HIVE_INSTALL/bin
export HADOOP_USER_CLASSPATH_FIRST=true

# hbase path
export HBASE_PATH=/usr/local/Cellar/hbase/1.0.1
export PATH=$PATH:$HBASE_PATH/bin

# storm path
export STORM_PATH=/usr/local/Cellar/storm/0.9.4
export PATH=$PATH:$STORM_PATH/bin

# protobuf path
export PROTOBUF_PATH=/usr/local/Cellar/protobuf/2.6.1
export PATH=$PROTOBUF_PATH/bin:$PATH

# go path
export GOPATH=/Users/yuanlang/go/workspace
export GOROOT=/usr/local/go

# akka path
export AKKA_PATH=/usr/local/Cellar/akka/2.3.11
export PATH=$AKKA_PATH/bin:$PATH

# tomcat path
export CATALINA_HOME=/usr/local/tomcat
export PATH=$CATALINA_HOME/bin:$PATH

alias jdk6='export JAVA_HOME=$JAVA_6_HOME'
alias jdk7='export JAVA_HOME=$JAVA_7_HOME'
alias jdk8='export JAVA_HOME=$JAVA_8_HOME'
CLASSPATH="$JAVA_HOME/lib"
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
PATH=".:$PATH:$JAVA_HOME/bin"
export M2_HOME=/usr/local/maven/apache-maven-3.2.2
export PATH=$PATH:$M2_HOME/bin
export JAVA_HOME
#export CLASSPATH
export M2_HOME
export LC_ALL=C

export PATH=/opt/subversion/bin:$PATH


################CLACSSPATH#################

# Java SDK
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
# Hadoop
CLASSPATH=$CLASSPATH:$HADOOP_HOME/lib/*:.
# Hive
CLASSPATH=$CLASSPATH:$HIVE_INSTALL/libexec/*:.

################CLACSSPATH#################

export CLASSPATH

source $ZSH/oh-my-zsh.sh

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# ssh
# export SSH_KEY_PATH="~/.ssh/dsa_id"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"
alias ll='ls -ail'
alias l='ls -ail'
alias svn='/opt/subversion/bin/svn'
alias phpdir='cd /Library/WebServer/Documents/ && ll'
alias mysqllogin='mysql -uroot -p123456'

#maven command alias
alias minst='mvn install -Dmaven.test.skip=true'
alias mcc='mvn clean compile -U'
alias mci='mvn clean install -Dmaven.test.skip=true'
alias mpkg='mvn package -Dmaven.test.skip=true'
alias md='mvn deploy -Dmaven.test.skip=true'
#maven command alias

alias cls='clear'
alias ll='ls -l'
alias la='ls -a'
alias vi='vim'
alias javac="javac -J-Dfile.encoding=utf8"
alias grep="grep --color=auto"
alias -s html=mate   # 在命令行直接输入后缀为 html 的文件名，会在 TextMate 中打开
alias -s rb=mate     # 在命令行直接输入 ruby 文件，会在 TextMate 中打开
alias -s py=vi       # 在命令行直接输入 python 文件，会用 vim 中打开，以下类似
alias -s js=vi
alias -s c=vi
alias -s java=vi
alias -s txt=vi
alias -s gz='tar -xzvf'
alias -s tgz='tar -xzvf'
alias -s zip='unzip'
alias -s bz2='tar -xjvf'
alias 103ssh='ssh yuanlang@10.11.2.103 -p10022'
alias 23ssh='ssh yuanalng@10.13.0.23 -p10022'
alias 1517ssh='ssh yuanlang@10.13.15.17 -p10022'
alias gmom="git merge origin/master"
alias to='/etc/profile.d/to'

LANG="en_US.UTF-8"
LANGUAGE="en_US:en"


alias jx126='ssh yuanlang@10.13.128.126 -p10022'

#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="/Users/yuanlang/.sdkman"
[[ -s "/Users/yuanlang/.sdkman/bin/sdkman-init.sh" ]] && source "/Users/yuanlang/.sdkman/bin/sdkman-init.sh"

export PATH="$PATH:$HOME/.rvm/bin" # Add RVM to PATH for scripting
