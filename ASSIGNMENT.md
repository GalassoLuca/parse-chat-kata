# Split the chat phrases

The following is a **TDD Kata**, an exercise in _coding_, _refactoring_ and _test-first_.

## Description

The goal is to parse a string chat: given a string input, the program should produce an array of sentences as output, according to the following specification.

## Before you start

- Try not to read ahead, do one task at a time!
- Make sure you only test for correct inputs;
- You may use whatever programming language you prefer;
- You should commit your code on GitHub (or any other SCM repository you prefer);
- You should release your work under an OSI-approved open-source license of your choice;

## What is a chat?

In our system, one of the data structure we have is the chat structure.

A chat is a list of sentences, diveded by the hour in which the message was written, followed by the name of the author, and the sentence at the end.

Here is the most complete example of a chat:

```
14:24:32 Customer : Lorem ipsum dolor sit amet, consectetur adipiscing elit.
14:26:15 Agent : Aliquam non cursus erat, ut blandit lectus.
14:27:00 Customer : Pellentesque cursus maximus felis, pharetra porta purus aliquet viverra.
14:27:47 Agent : Vestibulum tempor diam eu leo molestie eleifend.
```

We can say that:

- each piece of the chat is composed by `hh:mm:ss`, `customer/agent name`, `:` and `sentence`
- the first occurrence is from the customer
- the second occurrence could be either customer or agent

## The Kata

**Note**: for simplicity the following examples use "Customer" as customer name, and "Agent" as agent name, but they might be random strings like "Luca", "Irene" or even "name surname"

### Step 1 (single sentence)

_note:_ an example only with a sentence

Given the input

```
14:24:32 Customer : Lorem ipsum dolor sit amet, consectetur adipiscing elit.
```

The output should be

```
[{
  date: '14:24:32',
  mention: '14:24:32 Customer : ',
  sentence: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
  type: 'customer'
}]
```

### Step 2 (two sentences)

_note:_ an example with two sentences divided by new line character

Given the input

```
14:24:32 Customer : Lorem ipsum dolor sit amet, consectetur adipiscing elit.
14:26:15 Agent : Aliquam non cursus erat, ut blandit lectus.
```

The output should be

```
[{
  date: '14:24:32',
  mention: '14:24:32 Customer : ',
  sentence: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.\n',
  type: 'customer'
}, {
  date: '14:26:15',
  mention: '14:26:15 Agent : ',
  sentence: 'Aliquam non cursus erat, ut blandit lectus.',
  type: 'agent'
}]
```

### Step 3 (full chat)

_note:_ an example with more sentences from Customer and Agent

Given the input

```
14:24:32 Customer : Lorem ipsum dolor sit amet, consectetur adipiscing elit.
14:26:15 Agent : Aliquam non cursus erat, ut blandit lectus.
14:27:00 Customer : Pellentesque cursus maximus felis, pharetra porta purus aliquet viverra.
14:27:47 Agent : Vestibulum tempor diam eu leo molestie eleifend.
```

The output should be

```
[{
  date: '14:24:32',
  mention: '14:24:32 Customer : ',
  sentence: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.\n',
  type: 'customer'
}, {
  date: '14:26:15',
  mention: '14:26:15 Agent : ',
  sentence: 'Aliquam non cursus erat, ut blandit lectus.\n',
  type: 'agent'
}, {
  date: '14:27:00',
  mention: '14:27:00 Customer : ',
  sentence: 'Pellentesque cursus maximus felis, pharetra porta purus aliquet viverra.\n',
  type: 'customer'
}, {
  date: '14:27:47',
  mention: '14:27:47 Agent : ',
  sentence: 'Vestibulum tempor diam eu leo molestie eleifend.',
  type: 'agent'
}]
```

### Step 4 (two customer mentions as start)

_note:_ an example with two customer mentions as start

Given the input

```
14:24:32 Customer : Lorem ipsum dolor sit amet, consectetur adipiscing elit.
14:27:00 Customer : Pellentesque cursus maximus felis, pharetra porta purus aliquet viverra.
14:27:47 Agent : Vestibulum tempor diam eu leo molestie eleifend.
```

The output should be

```
[{
  date: '14:24:32',
  mention: '14:24:32 Customer : ',
  sentence: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.\n',
  type: 'customer'
}, {
  date: '14:27:00',
  mention: '14:27:00 Customer : ',
  sentence: 'Pellentesque cursus maximus felis, pharetra porta purus aliquet viverra.\n',
  type: 'customer'
}, {
  date: '14:27:47',
  mention: '14:27:47 Agent : ',
  sentence: 'Vestibulum tempor diam eu leo molestie eleifend.',
  type: 'agent'
}]
```

### Step 5 (date splitting)

_note:_ an example in which the sentences are not divided by the new line character

Given the input

```
14:24:32 Customer : Lorem ipsum dolor sit amet, consectetur adipiscing elit.14:26:15 Agent : Aliquam non cursus erat, ut blandit lectus.
```

The output should be

```
[{
  date: '14:24:32',
  mention: '14:24:32 Customer : ',
  sentence: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
  type: 'customer'
}, {
  date: '14:26:15',
  mention: '14:26:15 Agent : ',
  sentence: 'Aliquam non cursus erat, ut blandit lectus.',
  type: 'agent'
}]
```

### Step 6 (ignore extra dates)

_note:_ an example with a date in the text of the Agent

Given the input

```
14:24:32 Customer : Lorem ipsum dolor sit amet, consectetur adipiscing elit.14:26:15 Agent : I received it at 12:24:48, ut blandit lectus.
```

The output should be

```
[{
  date: '14:24:32',
  mention: '14:24:32 Customer : ',
  sentence: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
  type: 'customer'
}, {
  date: '14:26:15',
  mention: '14:26:15 Agent : ',
  sentence: 'I received it at 12:24:48, ut blandit lectus.',
  type: 'agent'
}]
```

### Step 7 - Extra (missing colon after the names)

_note:_ an example in which there is no colon after both Agent and Customer names

Given the input

```
14:24:32 Customer Lorem ipsum dolor sit amet, consectetur adipiscing elit.14:26:15 Agent I received it at 12:24:48, ut blandit lectus.
```

The output should be

```
[{
  date: '14:24:32',
  mention: '14:24:32 Customer ',
  sentence: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
  type: 'customer'
}, {
  date: '14:26:15',
  mention: '14:26:15 Agent ',
  sentence: 'I received it at 12:24:48, ut blandit lectus.',
  type: 'agent'
}]
```

## General requirements

The goal is to support the cases listed above in the best way you can. 

**IMPORTANT:** Provide a README with instructions on how to compile and run the application.

**CODE SUBMISSION:** Add the code to your own Github account and send us the link.
