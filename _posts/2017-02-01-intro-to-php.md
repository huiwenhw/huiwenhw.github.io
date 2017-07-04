---
layout: post
title: Intro to PHP 
category: normal
---
  
I had to read a bunch of PHP codes for one of my modules, so I decided to learn the basics of PHP from codecademy! This post will basically be a journal of syntax and of PHP. Skip if not interested! 

<br>
"PHP is an OOP language. PHP runs on the same computer as the website you're visiting, which is known as the server. This means that it has access to all the information and files on that machine, which allows it to construct custom HTML pages to send to your browser, handle cookies, and run tasks or perform calculations with data from that website."

.php: Tells PHP interpreter that there's PHP code in file to evaluate 

Syntax: Requires ; at end of every line 

Comments: Using // 

<br>
<p> Using echo for output, '.' for concatenation, $ for variables and conditional statements </p>
```
<?php
	echo "I'm learning" . " " . "PHP!";
	echo 17 * 123;
	echo "<p> We can use a {$var} inside echo too! </p>";
	// Output: I'm learning PHP! && 2091 

	if($myAge == 21) {
		echo "Happy 21st!";
	} elseif($myAge > 20) {
		echo "You're getting old"; 
	} else { ... } 
?>
```
<br>
<p> Switch Statements </p>
```
<?php 
	case $i = 5;
	switch($i) { *or* switch($i): 
		case 0:
			echo "0";
			break;
		case 1: 
		case 2:
		case 3:
			echo "1-3. Falling through";
			break;
		default: 
			echo "Don't know what number $i is";
	} *or* endswitch; 
?>
```
<br>
<p> On arrays. How to init, modify and echo with [] & {} </p>
```
<?php 
	$snacks = array("potato chips", "jagabee");
	echo $snacks[0] *or* {0}; 
	$snacks[0] = "pistachio"; 

	// Array. Deleting array element & whole array 
	unset($snacks[0]);
	unset($snacks);
?>
```
<br>
<p> On loops and arrays. (For loops and ForEach) </p>
```
<?php
	// For loops
	for($i = 10; $i <= 100; $i = $i + 10) {
		echo $i;
	}

	// ForEach 
	$numbers = array(1, 2, 3, 4, 5);
	foreach($numbers as $num) { 
		echo $num . " ";
		// output = 1 2 3 4 5 
	} 
	// Another representation of ForEach
	foreach($numbers as $num): 
		echo $num . " ";
		// output = 1 2 3 4 5 
	endforeach;
?>
```
<br>
<p> On while loops: Checks cond first </p>
```
<?php 
	$loopCond = true;
	while($loopCond) {  
		echo "<p>The loop is running.</p>";
		$loopCond = false;
	}
	// Another rep 
	while($loopCond): 
		echo "<p>The loop is running.</p>";
		$loopCond = false;
	endwhile;
?>
```
<br>
<p> On do-while loops: Checks cond aft each iteration </p>
```
<?php
	$loopCond = false;
	do {
		echo "<p> Loop will run once even if $loopCond is false </p>";
	} while ($loopCond);
?>
```
<br>
<p> Functions: String </p>
```
<?php
	$name = "huiwen";
	// substring. output: hui
	echo substr($name, 0, 3); 

	// uppercase. output: HUIWEN 
	echo strtoupper($name);

	// lowercase. output: huiwen
	echo strtolower($name);

	// strpos returns index of found char 
	if(strpos("huiwen", 'q') == false) {
		echo "Sorry, no q found";
	}
?>
```
<br>
<p> Functions: Math </p>
```
<?php
	// Rounding number to int/decimal place. output: 3 & 3.142 
	print round(M_PI);
	print round(M_PI, 3);

	// Rand num. rand(): 0 - 32767
	print rand(); 	
	print rand(min, max);
?>
```
<br>
<p> Functions: Array </p>
```
<?php
	// Pushing elements into array
	$randstr = array();
	array_push($randstr, "sup");
	array_push($randstr, "woop");

	// Count 
	print count($randstr);

	// Sort, Reverse-sort & join(glue, arr)
	$arr = array(5,4,7,6,1,2,9,0,8,3);
	sort($arr);
	print join(", ", $arr);		// 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
	rsort($arr);
	print join(", ", $arr);		// 9, 8, 7, 6, 5, 4, 3, 2, 1, 0
?>
```
<br>
<p> Writing our own functions </p>
```
<?php
	function aboutMe($name, $age) {
		echo "Hello! My name is {$name}, and I am {$age} years old.";
	}
	aboutMe("hw", 23);
	// output: Hello! My name is hw, and I am 23 years old.
?>
```
<br>
<p> Objects in PHP: Object Oriented Programming </p>
<p> Class; instance; methods; object; Properties: pieces of data bound to an object </p>
```
<?php
	// Creating a class 
	class Person {
		// adding properties 
		public $isAlive = true;
		public $name; 
	}

	// Creating an instance
	$student = new Person();
	print $student->isAlive;	// output: 1
?>
```
