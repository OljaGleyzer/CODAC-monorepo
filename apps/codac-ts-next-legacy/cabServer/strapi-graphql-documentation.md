<div align="center">
  <img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/images/strapql.png" />
  <h3 align="center">Strapi GraphQL API Documentation</h3>

  <p align="center">
    Collections of GraphQL queries and mutations that power your Strapi app!
    <br />
    <a href="https://github.com/kevinadhiguna/strapi-graphql-documentation#-table-of-contents"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/kevinadhiguna/strapi-graphql-documentation/issues">Report Bug</a>
    ·
    <a href="https://github.com/kevinadhiguna/strapi-graphql-documentation/issues">Request Feature</a>
  </p>
</div>
<br>

<!-- TABLE OF CONTENTS -->

## 📚 Table of Contents

1. [About Strapql](#-about-strapql)
2. [Queries and Mutations](#-queries-and-mutations)
   - [Register](#%EF%B8%8F-register)
   - [Login](#-login)
   - [Me Query](#-me-query)
     - [How to attach JWT in headers](#-how-to-attach-jwt-in-headers-)
   - [Create a user in Users](#-create-a-user-in-users-a-collection-type-that-comes-default-in-strapi-app)
     - [How to get Superadmin's JWT](#-how-to-get-superadmins-jwt)
   - [Retrieve/Fetch a single User](#-retrievefetch-a-single-user)
   - [Retrieve/Fetch all Users](#-retrievefetch-all-users)
   - [Update a User](#-update-a-user)
   - [Delete/Remove a User](#-deleteremove-a-user)
   - [Create an Entry in a Collection Type](#-create-an-entry-in-a-collection-type)
   - [Fetch/Retrieve a single entry in collection type](#-fetchretrieve-a-single-entry-in-collection-type)
   - [Fetch/Retrieve all entries in collection type](#-fetchretrieve-all-entries-in-collection-type)
   - [Update an entry in collection type](#-update-an-entry-in-collection-type)
   - [Delete/Remove an entry in collection type](#-deleteremove-an-entry-in-collection-type)
   - [Upload a single image](#-%EF%B8%8F-upload-a-single-image)
   - [Upload multiple images in a single field](#-%EF%B8%8F-upload-multiple-images-in-a-single-field)
   - [Upload a single image in separate fields](#-%EF%B8%8F-upload-a-single-image-in-separate-fields)
   - [Get all files](#-get-all-files)
   - [Fetch a single role](#-fetch-a-single-role)
   - [Fetch all roles](#---fetch-all-roles)
3. [Contributing](#-contributing)
4. [Contact](#-contact)
5. [Acknowledgements](#-acknowledgements)

<!--
<ol>
  <li>
    <a href="#-about-strapql">About Strapql</a>
  </li>
  <li>
    <a href="#-queries-and-mutations">Queries and Mutations</a>
  </li>
  <li><a href="#-contributing">Contributing</a></li>
  <li><a href="#-contact">Contact</a></li>
  <li><a href="#-acknowledgements">Acknowledgements</a></li>
</ol>
-->

## 🌟 About Strapql

Hello there, welcome to Strapi GraphQL API documentation! This contains some of queries and mutations that hopefully helps you if you are using GraphQL API in your Strapi project :)

## 🌈 Queries and Mutations

- Queries are used to read or fetch values while a mutation is used to write or post values (`READ`/`RETRIEVE`).
- Mutations modify data in the data store and returns a value. It can be used to insert, update, or delete data (`CREATE`, `UPDATE`, and `DELETE`).
  <br> (Source : [TutorialsPoint](https://www.tutorialspoint.com/graphql/index.htm))

## ®️ Register

Just like any other applications that requires you to create an account, you have to sign up first to create a user in `users` collection type that comes default in Strapi. Here is how to register an account :

```
mutation Register($input: UsersPermissionsRegisterInput!) {
  register(input: $input) {
    jwt
    user {
      username
      email
    }
  }
}
```

Next, put your `username`, `email`, and `password` as variables :

```
{
  "input": {
    "username": "YOUR_USERNAME",
    "email": "YOUR_EMAIL",
    "password": "YOUR_PASSWORD"
  }
}
```

Finally, a JWT shows in response.

## 🔒 Login

```
mutation Login($input: UsersPermissionsLoginInput!) {
  login(input: $input) {
    jwt
    user {
      username
      email
      confirmed
      blocked
      role {
        id
        name
        description
        type
      }
    }
  }
}
```

Then enter your `identifier` and `password` as variables :

```
{
  "input": {
    "identifier": "YOUR_USERNAME OR YOUR EMAIL",
    "password": "YOUR_PASSWORD"
  }
}
```

Eventually, you will get JWT in response.

## 🙋 Me Query

To identify current user, you can use `me` query, like this :

```
query MeQuery {
  me {
    id
    username
    email
    confirmed
    blocked
    role {
      id
      name
      description
      type
    }
  }
}
```

<b>Note : `me` query requires JWT attached in headers!</b>

### 📎 How to attach JWT in headers

`authorization : Bearer YOUR_TOKEN`

## 🆕 Create a User in Users (a collection type that comes default in Strapi app)

> What? Create a User? Did I just create a User using `Registration` mutation above?

Sure, here is some notable points :
| | `Create User` mutation | `Registration` mutation |
|:------------------------------------------:|:----------------------------------------------------:|:-------------------------------------------------------------------------------------------:|
| Needs JWT attached in Headers? | Yes, usually you must be `superadmin` role in Strapi | No |
| Is created user `authenticated` initially? | No | Yes, users that are created with `Registration` mutation is already authenticated initially |

```
mutation CreateUser($input: createUserInput) {
  createUser(input: $input) {
    user {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        id
        name
        description
        type
        permissions {
          type
          controller
          action
          enabled
          policy
          role {
            name
          }
        }
        users {
          username
        }
      }
    }
  }
}
```

Pass these variables :

```
{
  "input": {
    "data": {
      "username": "YOUR_USERNAME",
      "email": "YOUR_EMAIL",
      "password": "YOUR_PASSWORD"
    }
  }
}
```

<b>Note : Please attach a JWT in Headers, usually Superadmin's JWT.</b>

### 🔑 How to get Superadmin's JWT

Go to `Documentation` in the menu on the left side -> Copy the token in `Retrieve your jwt token`.

## 🧑 Retrieve/Fetch a single User

Previously, we created a new user. To retrieve a specific user inside User collection type, you can make use of this query :

```
query FetchSingleUser($id: ID!) {
  user(id: $id) {
    id
    createdAt
    updatedAt
    username
    email
    provider
    confirmed
    blocked
    role {
      name
    }
  }
}
```

Variables :

```
{
  "id": "YOUR_USER_ID"
}
```

## 👥 Retrieve/Fetch all Users

If you want to get all users in your Strapi app, this is the query you are looking for :

```
query FetchUsers {
  users {
    id
    createdAt
    updatedAt
    username
    email
    provider
    confirmed
    blocked
    role {
      name
    }
  }
}
```

You do not have to pass any variables but you may need to attach JWT in your headers (depends on your Strapi app's roles & permissions).

## 🔄 Update a User

Imagine you want to change a user's email. To do such things, you should use a mutation which updates the user's data. Here is an example to change a user's email :

```
mutation UpdateUser($input: updateUserInput) {
  updateUser(input: $input) {
    user {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

Then pass some variabes that you would like to change (in this case, `email` field) :

```
{
  "input": {
    "where": {
      "id": "YOUR_USER_ID"
    },
    "data": {
      "email": "YOUR_USER_EMAIL"
    }
  }
}
```

If you want to change fields other than `email`, just replace the `email` variable.

## ❌ Delete/Remove a User

> A user decided to no longer use my app. How do I remove him/her?

Here is a mutation that might do the task :

```
mutation deleteUser($input: deleteUserInput) {
  deleteUser(input: $input) {
    user {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

Place the user ID of the user you want to remove as a variable :

```
{
  "input": {
    "where": {
      "id": "YOUR_USER_ID"
    }
  }
}
```

<b>Note : Please carefully control which roles are able to conduct `delete` operation as it is sensitive.</b>

## 🆕 Create an Entry in a Collection Type

Suppose you have created a collection type named `idCardVerification`. Here is how you can add a new record inside it :

```
mutation createIdCardVerification($input: createIdCardVerificationInput) {
  createIdCardVerification(input: $input) {
    idCardVerification {
      id
      identifier
      birthPlace
    }
  }
}

```

For instace, `identifier` and `birthPlace` are variables available in `idCardVerification` collection type. Here are variables you should pass :

```
{
  "input": {
    "data": {
      "identifier": "YOUR_USERNAME OR YOUR_EMAIL",
      "birthPlace": "London, United Kingdom"
    }
  }
}
```

<b>Note : `birthPlace: London, United Kingdom` is just an example to fill a field</b>

## 📮 Fetch/Retrieve a single entry in collection type

To fetch an entry in your collection type, this query is probably able help you :

```
query FetchSingleIdCardVerification($id: ID!) {
  idCardVerification(id: $id) {
    id
    identifier
    birthPlace
  }
}
```

Pass the ID of the record/entry you want to fetch :

```
{
  "id": "ID_OF_ENTRY"
}
```

## 📒 Fetch/Retrieve all entries in collection type

This may get you all of the entries in your collection type :

```
query FetchIdCardVerifications {
  idCardVerifications {
    id
    identifier
    birthPlace
  }
}
```

## 🔄 Update an entry in collection type

```
mutation UpdateIdCardVerification($input: updateIdCardVerificationInput) {
  updateIdCardVerification(input: $input) {
    idCardVerification {
      id
      kind
      identifier
      birthPlace
    }
  }
}
```

You want to change `birthPlace` value to California, United States. Pass these in variables :

```
{
  "input": {
    "where": {
      "id": "ID_OF_ENTRY"
    },
    "data": {
      "birthPlace": "California, United States"
    }
  }
}
```

You are changing `birthPlace` field. The response should display `birthPlace` field with the updated value.

## ❌ Delete/Remove an entry in collection type

```
mutation deleteIdCardVerification($input: deleteIdCardVerificationInput) {
  deleteIdCardVerification(input: $input) {
    idCardVerification {
      id
      identifier
      birthPlace
    }
  }
}
```

Variables :

```
{
  "input": {
    "where": {
      "id": "ID_OF_ENTRY"
    }
  }
}
```

## 📤 🖼️ Upload a single image

### ⚠️ Warning : Currently Strapi's GraphQL Playground does not support file/image upload. You can use other GraphQL client to test your GraphQL upload mutation

One of the GraphQL clients I use is Altair. You can download it here : <https://altair.sirmuel.design/#download>

Please create a new entry in your collection type API first ! Otherwise this will not be attached to your entry.
Note : the `refId` is the ID of the entry you create in your collection type API.

```
mutation SingleImageUpload($refId: ID, $ref: String, $field: String, $file: Upload!) {
  upload(refId: $refId, ref: $ref, field: $field, file: $file) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Variables :

```
{
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE",
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "field": "FIELD_NAME"
}
```

Here is an example :<br/>
<img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/gif/singleImageUpload.gif" />

## 📤 🖼️ Upload multiple images in a single field

```
mutation MultipleImageUpload(
  $refId: ID
  $ref: String
  $field: String
  $files: [Upload]!
) {
  multipleUpload(refId: $refId, ref: $ref, field: $field, files: $files) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Variables :

```
{
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE",
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "field": "FIELD_NAME"
}
```

<b>Note : In this case, I attached images with name `files.0`, `files.1`, ... , `files.n` as variables' names until the number of image you want to upload (n).</b>

Here is an example :<br/>
<img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/gif/multipleImageUpload.gif" />

## 📤 🖼️ Upload a single image in separate fields

> Hmm... but how do I upload a single image to several fields in a single request?

All right, imagine you created a collection type which has several fields, including `cardImage`, `facePhoto`, and `personWithCardPhoto`. Otherwise, just replace those fields with yours. Ok, here we go :

```
mutation UploadSingleImageToSeveralFields(
  $ref: String
  $refId: ID
  $cardImage: Upload!
  $facePhoto: Upload!
  $personWithCardPhoto: Upload!
) {
  cardImage: upload(
    ref: $ref
    refId: $refId
    field: "cardImage"
    file: $cardImage
  ) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
  facePhoto: upload(
    ref: $ref
    refId: $refId
    field: "facePhoto"
    file: $facePhoto
  ) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
  personWithCardPhoto: upload(
    ref: $ref
    refId: $refId
    field: "personWithCardPhoto"
    file: $personWithCardPhoto
  ) {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Variables :

```
{
  "ref": "YOUR_COLLECTION_TYPE_NAME",
  "refId": "ID_OF_YOUR_ENTRY_IN_YOUR_COLLECTION_TYPE"
}
```

<b>Please do not forget to attach your files with variables' names.</b><br>
Note: In this case, the variables' names are `cardImage`, `facePhoto`, and `personWithCardPhoto`.

Here is an example :<br/>
<img src="https://raw.githubusercontent.com/kevinadhiguna/strapi-graphql-documentation/master/assets/gif/uploadSingleImageToSeveralFields.gif" />

### 🚀 How does `UploadSingleImageToSeveralFields` mutation work ?

In the `UploadSingleImageToSeveralFields` mutation above, you still need `ref`, `refId`, and field name. However you are sending a request to a collection type and are trying to attach images in a sngle record inside the collection type. So, you are able to set `ref` and `refId` as variables. The field name ? You should name it statically as you want to upload an image in different fields. Hopefully this approach helps :)

## 📂 Get all files

> All right, I got images and files uploaded to my Strapi app but how do I know what files did I upload ?

To get all the files uploaded to database within your Strapi app, here is the query :

```
query FetchFiles {
  files {
    id
    createdAt
    updatedAt
    name
    alternativeText
    caption
    width
    height
    formats
    hash
    ext
    mime
    size
    url
  }
}
```

Unfortunately, currently Strapi does not provide a query to fetch a single file.

## 👨‍💻 Fetch a single role

Here is the query to display a single role :

```
query fetchSingleRole($id: ID!) {
  role(id: $id) {
    id
    name
    description
    type
    permissions {
      id
      type
      controller
      action
      enabled
      policy
      role {
        name
      }
    }
    users {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

Variable :

```
{
  "id": "ROLE_ID"
}
```

## 👨‍💻 👨‍💼 🧑‍🔧 Fetch all roles

Below is the query to get all roles :

```
query FetchRoles {
  roles {
    id
    name
    description
    type
    permissions {
      id
      type
      controller
      action
      enabled
      policy
      role {
        name
      }
    }
    users {
      id
      createdAt
      updatedAt
      username
      email
      provider
      confirmed
      blocked
      role {
        name
      }
    }
  }
}
```

## 🖊 Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some Amazing Queries or Mutations'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🌐 Contact

Author : Kevin Adhiguna - [@kevinadhiguna](https://linkedin.com/in/kevinadhiguna) - hi.kevinadhiguna@gmail.com

See on Github : [https://github.com/kevinadhiguna/strapi-graphql-documentation](https://github.com/kevinadhiguna/strapi-graphql-documentation)

Read more on Blog : [https://about.lovia.life/docs/engineering/graphql/strapi-graphql-documentation/](https://about.lovia.life/docs/engineering/graphql/strapi-graphql-documentation/)

## 🎉 Acknowledgements

- [Strapi](https://github.com/strapi/strapi)
- [Lovia](https://about.lovia.life)
- [Best-README-Template](https://github.com/othneildrew/Best-README-Template)
