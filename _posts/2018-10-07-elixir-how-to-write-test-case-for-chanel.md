---
layout: post
title:  "[Elixir] How to test for socket-channel?"
comments: true
date:   2018-03-12 15:07:33 +0700
categories: elixir
---

## Elixir yeah >___<

### How to write test case for socket-channel?

This blog I will share how to testing <b>Phoenix channels</b> and how to handle the channel with better code. Small demo and easy to understand.

I got problem when I writing the code for channel. I wrote the code to connect and return the data from the channel. It makes the code complex and hard to detect where was problem. I spent a lot of time to debugging and big problem here was I did not write tests for channel.

Techincally, test case cannot connect with channel. I spend time to research, find the way to create connection but it does not make sense. I asked my team, and he gave me a suggestion. His idea is separate the code, means I need to define the logic *in a service*  and return what channel wants. Channel will call the service, return and reply the data to Frontend. Damn!!! Good idea but I need to write test cases for Channel as well. 

His idea was good for me. Let's create a small demo follow his suggestion. There are 2 steps to handle:

**1) Create Service**
  - Define the business logic in a service
  - Write test case for this service

**2) Create Channel**
  - Init the channel
  - Write test case for channel

### Logic
To get started, I create a service called **KeyboardAppService** and `KeyboardAppServiceTest` to test.

I will test 4 cases:

1. `get_keyboard/1 with valid id`

2. `get_keyboard/1 with invalid id (2 case defined to make test for channel connect with valid keyboard_id and invalid keyboard_id.)`

3. `scan_keyboard/1 return correct data`

4. `update_brand/1 return correct data`

In this service I only assert `data_key` returns should correct because I already tested the `data_value` in another test case.

Third party can be a mobile app or website... To easy, It will be called "the App".

### Get the information: 

App need to join channel to get data.

```

{:ok, scan_data, socket} =
        socket("", %{
          device_id: "iphone",
          device_name: "janyIphone"
        })
        |> subscribe_and_join(KeyboardChannel, "keyboard:#{keyboard.id}")
        
```

Once subscribed and joined the process to the **"keyboard:#{keyboard.id}"** topic It returns 

**{:ok, reply, socket}** or **{:error, reply}**

So, we define the test case for:

1. join valid channel with return {:ok, reply, socket} and test for the scan_data return

2. Invalid chanel with return {:error, reply}.

### Update information: 

App need to communicate with server via `handle_in`. In test, we use `push` to communicate in socket.

```
ref = push(socket, "update_brand", %{id: keyboard.id, brand: "brand"})
```

and assert ref return :ok with data you want to update

I pushed whole the code on github: Tou can take a look at [`keyboard_chanel`](https://github.com/mymai91/keyboard_channel)

I used **ex_machina** library to create fake data, **mix_test_watch** to automate run  the test when I change the code and **excoveralls** to check code coverage.

### Run test case

```
mix test
```

Let see 2 cases I wrote: 100% coverage >___<

#### Keyboard app service
![screen shot 2018-10-06 at 11 07 13 am](https://user-images.githubusercontent.com/6791942/46582993-4101db00-ca82-11e8-9b3a-f6c5c59a9fdb.png)

#### Keyboard channel
![screen shot 2018-10-06 at 11 06 52 am](https://user-images.githubusercontent.com/6791942/46582990-32b3bf00-ca82-11e8-8657-3ab4146b9454.png)


*Read comment code to get more understand*

Check code detail at: [`keyboard_chanel`](https://github.com/mymai91/keyboard_channel)


To start your Phoenix server:

  * Install dependencies with `mix deps.get`
  * Create and migrate your database with `mix ecto.create && mix ecto.migrate`
  * Install Node.js dependencies with `cd assets && npm install`
  * Start Phoenix endpoint with `mix phx.server`

Now you can visit [`localhost:4000`](http://localhost:4000) from your browser.

`open cover/excoveralls.html` from terminal

### Aside

Have you ever says that "we are lucky to have the test case in project"?
never - often - usually or always

How about me and my partner?
The answer is always, always and always. So, why???

Project will bigger and bigger day by day, accompany the complexion business logic significantly increase. There are situations you forgot tiny case and it makes your code broken. You might or might not figure out when you do manually test on browser.

Do not let end-user blame your system (Actually they blamed you :d). Limit it as much as possible and control it as soon as possible. Okay, let's always write tests. This is the best way to control the 90% the risk.

You **NEED TO** or **HAVE TO** or **MUST TO** write

**How about the question:**

Write test first or write code first. My answer is depends. 

Some case I want to know the result then I will write code first, some very very complex case I choose write test first... Answer is depend.


^___^

Jany Mai
