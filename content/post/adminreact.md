---
title: "Admin Page on React"
date: 2018-03-21T11:58:07+07:00
author: Felicia
tags: ["Felicia", "React", "Sprint 1"]
---

The task for this week is creating admin app. Currently the view is a prototype mocked from online API mocker, since the backend is not ready yet to be displayed. This post is going to be step by step setting up react until we reach this view

![Interface Image](/img/adminabeona.png)

# Init everything
For rapid development purposes we will use several dependencies to develop the interface

Faster node project creation using create-react-app
```bash
npm i -g create-react-app
create-react-app abeona-admin
```

Testing using Jest, since it is the testing framework used by google
```bash
npm i --save-dev jest
```

For faster interface and view
```bash
npm i admin-on-rest
```

# To create

The JSON server to mock the API is `http://jsonplaceholder.typicode.com` they only provide popular endpoint like posts and users, so those are what we are going to use

The main code to fetch rest utilizing dependency to access the mock API

```xml
<Admin
  title="Admin Abeona"
  restClient={jsonServerRestClient('http://jsonplaceholder.typicode.com')}>
    <Resource name="posts" list={PostList} />
    <Resource name="users" list={UserList} />
</Admin>
```

For every resource fetched the datagrid must be defined. Here is the datagrid for UserList
```xml
<List title="All users" {...props}>
    <Datagrid>
        <TextField source="id" />
        <TextField source="name" />
        <TextField source="username" />
        <EmailField source="email" />
    </Datagrid>
</List>
```

# Testing

Currently no unit testing is made, basic jest testing can be run by
```bash
npm test
```

# To Do
* Create the CRUD functionability
