#!/bin/bash
# todo.txt support add-on for markdown support
# Copyright (c) Lal Thomas, http://lalamboori.blogspot.in
# License: GPL3, http://www.gnu.org/copyleft/gpl.html


printTrailingCharacter(){

 	character=$1
	# markdown heading label
 	COUNTER=0
 	while [  $COUNTER -lt $length ]; do
		printf '%s' $character >>"$2"
	    let COUNTER=COUNTER+1 
	  done
}

add(){

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
		printf "### " >>"$filePath"
		printf "$title" >>"$filePath"
		printf "\n\n" >>"$filePath"
		;;		
	H4) 
		# Heading IV		
		printf "#### " >>"$filePath"
		printf "$title" >>"$filePath"
		printf "\n\n" >>"$filePath"
		;; 	
	p) 
		printf "$title" >>"$filePath"
		printf "\n\n" >>"$filePath"
		;;
	c) 
		printf "`$title`" >>"$filePath"
		printf "\n\n" >>"$filePath"
		;;
	em)
		printf "*$title*" >>"$filePath"		
		;;	
	li)
		printf -- "- $title" >>"$filePath"		
		;;			
	yaml)
		printf "%% $title" >>"$filePath"
		printf "\n" >>"$filePath"
		;;		
	blank)
		printf "\n" >>"$filePath"
		;;		
	*) 
		# Heading Unknown		
		echo "unknown markup type" 
		;; 	
		
  esac  
  
}

usage(){
# list usage information
		echo "    markdown"    
		echo "      (no arguments)   - view usage"	
}

shift
ACTION=$1

case "$ACTION" in

	"usage")
		usage
		exit	
	;;
	
	"add")		
		shift
		if [[ -z "$3" ]]; then
		   echo "markdown error : few arguments"			
		else		   
		   # "$1" - markup
		   # "$2" - title
		   # "$3" - filePath
		   add "$1" "$2" "$3"
		fi	
		exit
	;;
	
	*)
	# list planner file	
	 echo
	 echo "arguments - $@ - not valid" 
	 echo 
	 usage
	 exit
	;;	   
esac
