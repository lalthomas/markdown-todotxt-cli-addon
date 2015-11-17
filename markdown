#!/bin/bash
# addon for markdown support for other addons
#
# Copyright (C) 2013 Lal Thomas
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

printTrailingCharacter(){

 	character=$1
	# markdown heading label
 	COUNTER=0
 	while [  $COUNTER -lt $length ]; do
		printf '%s' $character >>"$2"
	    let COUNTER=COUNTER+1 
	  done
}

addMarkdown(){

  local markup=$1
  local title=$2
  local filePath=$3
  local length=${#title} 
  
  case $markup in
	H1) 
		# Heading I		
		printf "$title" >>"$filePath"
		printf "\n" >>"$filePath" 
		printTrailingCharacter '=' "$filePath"
		# add two blank line
		printf "\n\n" >>"$filePath"     		
		;;
	H2) 
		# Heading II		
		printf "$title" >>"$filePath"
		printf "\n" >>"$filePath" 
		printTrailingCharacter '-' "$filePath"
		# add two blank line
		printf "\n\n" >>"$filePath"		
		;; 
	H3) 
		# Heading III		
		printf "###" >>"$filePath"
		printf "$title" >>"$filePath"
		printf "\n\n" >>"$filePath"
		;;		
	H4) 
		# Heading IV		
		printf "####" >>"$filePath"
		printf "$title" >>"$filePath"
		printf "\n\n" >>"$filePath"
		;; 	
	p) 
		printf "$paraTitle" >>"$filePath"
		printf "\n\n" >>"$filePath"
	;;
	c) 
		printf "`$paraTitle`" >>"$filePath"
		printf "\n\n" >>"$filePath"
	;;
	*) 
		# Heading Unknown		
		echo "unknown markup type" 
	;; 
	
  esac  
  
}

adddonUsage(){
# list usage information
		echo "    markdown"    
		echo "      (no arguments)   - view usage"	
}

shift
ACTION=$1

case "$ACTION" in

	"usage")
		adddonUsage
		exit	
	;;
	
	"add")		
		shift
		if [[ -z "$3" ]]; then
		   echo "error : few arguments"
		   #adddonUsage
		else		   
		   # "$1" - markup
		   # "$2" - title
		   # "$3" - filePath
		   addMarkdown "$1" "$2" "$3"
		fi	
		exit
	;;
	
	*)
	# list planner file	
	 echo
	 echo "arguments - $@ - not valid" 
	 echo 
	 adddonUsage
	 exit
	;;	   
esac
