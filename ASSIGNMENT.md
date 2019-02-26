# Split the chat phrases

## Chat description
1. all the sentences are created like `hh:mm:ss`  `customer/user name`  `:`  `sentence`
2. the first occurrence is from the customer
3. the second occurrence could be customer or agent

## Step 1
Given the input `today Luca : hello world.`the output should be like
```
[{
	text: 'today Luca : hello world.'
}]
```

## Step 2
Given the input
```
today Luca : hi there. \ntoday Andrea : hello
today Andrea: hi again... are you there? 
```
The output should be
```
[{
	text: 'today Luca : hi there. \n'
},{
	text: 'today Andrea : hello'
},{
	text: 'today Andrea: hi again... are you there?'
}]
```

## Step 3
 Step 2
Given the input
```
14:30:00 Luca : hi there. \14:30:35 Andrea : hello
14:45:18 Andrea: hi again... are you there? 
```
The output should be
```
[{
	text: '14:30:00 Luca : hi there. \n'
},{
	text: '14:30:35 Andrea : hello'
},{
	text: '14:45:18 Andrea: hi again... are you there?'
}]
```

## Step 4
Given the input
```
14:30:00 Luca : hi there. \14:30:35 Andrea : hello
14:45:18 Andrea: hi again... are you there? 
```
The output should be
```
[{
	date: '14:30:00',
	author: 'Luca',
	message: 'hi there. \n'
},{
	date: '14:30:35',
	author: 'Andrea',
	message: 'hello'
},{
	date: '14:45:18',
	author: 'Andrea',
	message 'hi again... are you there?'
}]
```

## Step 5
In the chat there are only 2 users
Given the input
```
14:30:00 Lerna ø A'sd : hi there

14:30:05 Andrea: I Lerna, how can i help you?

14:30:15 Lerna ø A'sd : I'm having troubles with my mac: it is completly dead.

14:30:35 Andrea : I am sorry for that. May you give me the serial number of your mac?

14:45:18 Lerna ø A'sd : really?? goodbye!!!!
```
The output should be
```
[{
	date: '14:30:00',
	author: 'Lerna ø A'sd',
	message: 'hi there'
}, {
	date: '14:30:05',
	author: 'Andrea'
	message: 'I Lerna, how can i help you?'
}, {
	date: '14:30:15'
	author: 'Lerna ø A'sd'
	message: 'I'm having troubles with my mac: it is completly dead.'
}, {
	date: '14:30:35',
	author: 'Andrea',
	message: 'I am sorry for that. May you give me the serial number of your mac?'
}, {
	date: '14:45:18',
	author: 'Lerna ø A'sd',
	message 'really?? goodbye!!!!'
}]
```


## Step 6 – edge cases
The name can’t have more than 25 characters
Given the input
```
14:30:00 Lerna ø A'sd : hi there, i'm luce and today at 14:30:01 i had a lot of issue using my mac, for example: it is completly dead. 14:30:35 Andrea : I am sorry for that. May you give me the serial number of your mac?
14:45:18 Lerna ø A'sd : really?? goodbye!!!!
```
The output should be
```
[{
	date: '14:30:00',
	author: 'Lerna ø A'sd',
	message: 'hi there, i'm luce and today at 14:30:01 i had a lot of issue using my mac, for example: it is completly dead.'
},{
	date: '14:30:35',
	author: 'Andrea',
	message: 'I am sorry for that. May you give me the serial number of your mac?'
},{
	date: '14:45:18',
	author: 'Lerna ø A'sd',
	message 'really?? goodbye!!!!'
}]
```

## Step 4bis
Given the input from 4, the output should be like:
```
[{
	date: '14:30:00',
	author: 'Luca',
	message: 'hi there. \n',
	from: 16,
	to: 27
},{
	date: '14:30:35',
	author: 'Andrea',
	message: 'hello',
	from: 27,
	to: 50
},{
	date: '14:45:18',
	author: 'Andrea',
	message 'hi again... are you there?',
	from: 50,
	to: 95
}]
```


