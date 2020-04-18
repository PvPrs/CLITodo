#!/bin/bash

CONF_FILE='default.conf'
RED='\033[0;31m'
GREEN='\033[0;32m'
BLUE='\033[0;34m'
NC='\033[0;0m'
BOLD='\033[1;1m'

commands=("-h" "-help" "help" "-n" "-new" "-note" "note" "--set-default" "-d" "ls" "-ls" "=l" "clear" "-clear" "-rm" "rm")

function add_component () { 
	for ((arg = 1 ; arg < "$#" ; arg++)); do
		echo "Adding ${RED}${!arg}${NC} to the ${BLUE}${BOLD}@TODO:${NC} list..."
		echo "${BOLD}IMPORTANCY / URGENCY: ${NC}"
		echo "use the ${GREEN}${BOLD}+${NC} operator to indicate the urgency"
		echo "or use a custom urgency rate."
		printf "${BOLD}${GREEN}+${NC}: "
		read -p "" urgency
		printf "${BLUE}@:${NC} ${!arg}%-40s${GREEN}\$$urgency\n" >> "$2"
		echo "${RED}${!arg}${NC} has been added to the ${BLUE}${BOLD}@TODO:${RED} Note: ${!#}."
	done
}

function print_manual () {
	echo "${BLUE}@TODO: Manual.${NC}"
	echo "USAGE: todo [What to do?]\n"
	printf -- "-n, -note, note, -new%-40sNew Notes\n" | column -c 2
	printf -- "-d, --set-default%-40sSet Default Note to show upon 'todo' command\n" | column -c 2
	printf -- "-ls%-40sList all available notes.\n" | column -c 2
	printf -- "-rm[arg]%-40sRemoves from ${BLUE}${BOLD}@TODO:${NC} list.\n" | column -c 2
	printf -- "-clear, clear%-40sClears the current 'default' note.\n" | column -c 2
	printf -- "-h, -help, help%-40sPrompts this useful manual.\n" | column -c 2
}

function create_note () {
	arg="$1"
	printf "${BLUE}${BOLD}@TODO:%-40s${RED}\$IMPORTANCE${NC}\n" > "$HOME"/@TODO/notes/"$arg"
	echo "${BLUE}@TODO: ${NC}${arg} has been added to your notes."
	echo "${BLUE}@TODO: ${NC}Use --set-default to set as default note."
}

function init_command () {
	case $1 in
		-h | -help | help )
			print_manual "$@"
			;;
		-n | -new | -note | note )
			create_note "$2"
			;;
		--set-default | -d )
			echo "default=${HOME}/@TODO/notes/${2}" > "$HOME"/@TODO/"$CONF_FILE"
			echo "${BLUE}@TODO:${NC} Default note set to ${2}"
			;;
		ls | -ls | -l )
			echo "${BLUE}@TODO${NC} NOTES: "
			ls "$HOME"/@TODO/notes/
			;;
		-clear | clear )
			printf "${BLUE}${BOLD}@TODO:%-40s${RED}\$IMPORTANCE${NC}\n" > "$FILE"
			echo "Cleared the ${BLUE}${BOLD}@TODO:${NC} list."
			;;
		-rm | rm )
			rm "$HOME"/@TODO/notes/"$2"
			echo "${BLUE}@TODO:${NC} Removed ${2}."
			;;
	esac
	exit
}

# $@ Represents all the arguments passed,
# $# Variable returns the nbr of input arguments
# $1 for example refers to the first argument

function init () {
	matchCommand=0
	if ! [ -d "$HOME"/@TODO ]; then
		FILE="$HOME"/@TODO/notes/todo
		mkdir "$HOME"/@TODO
		if ! [ -d "$HOME"/@TODO/notes ]; then
			mkdir "$HOME"/@TODO/notes
		fi
		touch "$HOME"/@TODO/notes/todo | echo "default="$HOME"/@TODO/notes/todo" >> "$HOME"/@TODO/"$CONF_FILE"
		printf "${BLUE}${BOLD}@TODO:%-40s${RED}\$IMPORTANCE${NC}\n" > "$FILE"
	fi
	FILE=$(cat "$HOME"/@TODO/"$CONF_FILE" | awk -F '=' '{print $2}')
	if ! [ -f "$FILE" ]; then
		create_note "$2"
	fi
	if [ "$#" -eq 0 ]; then
		cat "$FILE" | column -c 2
		printf "%-40s${NC}Help -h \ EOF\n" | column -c 2
		exit
	fi
	for i in "${!commands[@]}"; do
		if [ ${commands[i]} == "$1" ]; then
			init_command "$@"
		fi
	done
	add_component "$@" "$FILE"
}

init "$@"