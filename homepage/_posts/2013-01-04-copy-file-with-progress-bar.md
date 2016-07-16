---
layout: post
title: "Copy file with progress bar"
description:
category: linux
tags: [shell]
---

#### Copy file with progress bar  

	#!/usr/bin/bash

	print_bar() {		# print the bar 

		local Count=$1
		local Max=$2
		local Remain=$3
		local Elapsed=$4
		local Speed=$5
		local LenBar=50
		if [ $# -eq 6 ]; then
			LenBar=$6
		fi
		if [ ${Count} -gt ${Max} ]; then
			Count=${Max}
		fi
		local Percent=$(((100*Count)/Max))
		local BarPercent=$(((LenBar*Percent)/100))

		if [ ${Percent} -eq 100 ]; then
			Remain=0
		fi
		
		local BarProgress=$(for i in $(seq ${BarPercent}); do echo -n '='; done)
		BarProgress="${BarProgress:0:${#BarProgress}}>"
		local Nxt=$((LenBar-BarPercent))
		local BarPad=$(for i in $(seq ${Nxt}); do echo -n '.'; done)

		local TElapsed=$(date -u -d @$Elapsed +%Hh%Mm%Ss)
		TElapsed=$(echo ${TElapsed} | sed -e "s/^00h//" -e "s/^00m//")

		local TRemain=$(date -u -d @$Remain +%Hh%Mm%Ss)
		TRemain=$(echo ${TRemain} | sed -e "s/^00h//" -e "s/^00m//")

		local larg=$(tput cols)
		if [ ${TimeElapsed} -gt ${TimeDiff} ] && [ ${EndBar} -eq 0 ] && [ ${Remain} -gt 0 ]; then		
			local mess0="[ ${Percent}% ][${BarProgress}${BarPad}] (${Speed}K/s) [ Remain: ${TRemain} Spent: ${TElapsed} ]"
			local mess=" :: ${mess0}"
			local mess1="${TAB_}${mess0}"
			echo -en "\r${mess1}\033[K"
			if [ ${#mess} -gt ${larg} ]; then
				echo -ne "\r\033[1A" src/lib/progbar
			fi
		else
			echo -en "\r${TAB_}[ ${Percent}% ][${BarProgress}${BarPad}][ Time: ${TElapsed} ]\033[K"
		fi
	}

	if [ $# -eq 2 ]; then
		cp $1 $2 &
	fi

	file_size_org=0
	file_size_dst=0
	file_size_org=$(du $1 | awk '{print $1}')
	#echo org size ${file_size_org}

	TimeDiff=10
	EndBar=1
	TimeStart=$(date +%s)
	while [ ${file_size_org} -gt ${file_size_dst} ] && sleep 1
	do
		file_size_dst=$(du $2 | awk '{print $1}')
		TimeNow=$(date +%s)
		TimeElapsed=$(( TimeNow-TimeStart ))
		Speed=$(( file_size_dst/TimeElapsed ))
		TimeRemain=$(( (file_size_org-file_size_dst)/Speed ))
	#	echo dst size ${file_size_dst}
		print_bar ${file_size_dst} ${file_size_org} ${TimeRemain} ${TimeElapsed} ${Speed}
	#	print_bar ${BytesLoadNow} ${BytesLoadMax} ${TimeRemain} ${TimeElapsed} ${Speed} ${LenBar}
	done
	echo


#### Reference  
* [raider: src/lib/progbar](http://sourceforge.net/projects/raider/files/?source=navbar)
