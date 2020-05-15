```
DELETE /__TESTUTILS__/purge
```
```
200 OK

"Purged all data!"
```
# Article
```
POST /users

{
  "user": {
    "email": "author-yqr3kj@email.com",
    "username": "author-yqr3kj",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "author-yqr3kj@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvci15cXIza2oiLCJpYXQiOjE1ODk1ODU3OTgsImV4cCI6MTU4OTc1ODU5OH0.9h-1zcYHlnB4_I9VqZiCrmrsOLCnHioJnq3JUGit14I",
    "username": "author-yqr3kj",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "authoress-8ja3wy@email.com",
    "username": "authoress-8ja3wy",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "authoress-8ja3wy@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvcmVzcy04amEzd3kiLCJpYXQiOjE1ODk1ODU3OTgsImV4cCI6MTU4OTc1ODU5OH0.H7guI9MB0oSNDFRoEOXf9luC4wMSyGf9cS5ZpHO-EZw",
    "username": "authoress-8ja3wy",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "non-author-rb756c@email.com",
    "username": "non-author-rb756c",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "non-author-rb756c@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Im5vbi1hdXRob3ItcmI3NTZjIiwiaWF0IjoxNTg5NTg1Nzk4LCJleHAiOjE1ODk3NTg1OTh9.hmKZGV5nKtXOZVyJGIl-jf_-iGNV47Hhx-MsKTec0gk",
    "username": "non-author-rb756c",
    "bio": "",
    "image": ""
  }
}
```
## Create
### should create article
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body"
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-w95axy",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585798690,
    "updatedAt": 1589585798690,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should create article with tags
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "tag_a",
      "tag_b"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title--zco14d",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585798743,
    "updatedAt": 1589585798743,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should disallow unauthenticated user
```
POST /articles

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should enforce required fields
```
POST /articles

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "title must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "description must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "body must be specified."
    ]
  }
}
```
## Get
### should get article by slug
```
GET /articles/title-w95axy
```
```
200 OK

{
  "article": {
    "createdAt": 1589585798690,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-w95axy",
    "updatedAt": 1589585798690,
    "tagList": [],
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should get article with tags by slug
```
GET /articles/title--zco14d
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1589585798743,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title--zco14d",
    "updatedAt": 1589585798743,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should disallow unknown slug
```
GET /articles/xzzuok
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [xzzuok]"
    ]
  }
}
```
## Update
### should update article
```
PUT /articles/title--zco14d

{
  "article": {
    "title": "newtitle"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1589585798743,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "newtitle",
    "body": "body",
    "slug": "title--zco14d",
    "updatedAt": 1589585798743,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
```
PUT /articles/title--zco14d

{
  "article": {
    "description": "newdescription"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1589585798743,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "newdescription",
    "title": "newtitle",
    "body": "body",
    "slug": "title--zco14d",
    "updatedAt": 1589585798743,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
```
PUT /articles/title--zco14d

{
  "article": {
    "body": "newbody"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1589585798743,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "newdescription",
    "title": "newtitle",
    "body": "newbody",
    "slug": "title--zco14d",
    "updatedAt": 1589585798743,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should disallow missing mutation
```
PUT /articles/title--zco14d

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article mutation must be specified."
    ]
  }
}
```
### should disallow empty mutation
```
PUT /articles/title--zco14d

{
  "article": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "At least one field must be specified: [title, description, article]."
    ]
  }
}
```
### should disallow unauthenticated update
```
PUT /articles/title--zco14d

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow updating non-existent article
```
PUT /articles/foo-title--zco14d

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foo-title--zco14d]"
    ]
  }
}
```
### should disallow non-author from updating
```
PUT /articles/title--zco14d

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article can only be updated by author: [author-yqr3kj]"
    ]
  }
}
```
## Favorite
### should favorite article
```
POST /articles/title-w95axy/favorite

{}
```
```
200 OK

{
  "article": {
    "createdAt": 1589585798690,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-w95axy",
    "updatedAt": 1589585798690,
    "favoritedBy": [
      "non-author-rb756c"
    ],
    "favoritesCount": 1,
    "tagList": [],
    "favorited": true
  }
}
```
```
GET /articles/title-w95axy
```
```
200 OK

{
  "article": {
    "createdAt": 1589585798690,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "favoritesCount": 1,
    "slug": "title-w95axy",
    "updatedAt": 1589585798690,
    "tagList": [],
    "favorited": true
  }
}
```
### should disallow favoriting by unauthenticated user
```
POST /articles/title-w95axy/favorite

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow favoriting unknown article
```
POST /articles/title-w95axy_foo/favorite

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [title-w95axy_foo]"
    ]
  }
}
```
### should unfavorite article
```
DELETE /articles/title-w95axy/favorite
```
```
200 OK

{
  "article": {
    "createdAt": 1589585798690,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "favoritesCount": 0,
    "slug": "title-w95axy",
    "updatedAt": 1589585798690,
    "tagList": [],
    "favorited": false
  }
}
```
## Delete
### should delete article
```
DELETE /articles/title-w95axy
```
```
200 OK

{}
```
```
GET /articles/title-w95axy
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [title-w95axy]"
    ]
  }
}
```
### should disallow deleting by unauthenticated user
```
DELETE /articles/foo
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow deleting unknown article
```
DELETE /articles/foobar
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foobar]"
    ]
  }
}
```
### should disallow deleting article by non-author
```
DELETE /articles/title--zco14d
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article can only be deleted by author: [author-yqr3kj]"
    ]
  }
}
```
## List
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "kx1dys",
      "tag_0",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-obzifs",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799705,
    "updatedAt": 1589585799705,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "kx1dys",
      "tag_0",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "9w5l4l",
      "tag_1",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-187oly",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799748,
    "updatedAt": 1589585799748,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "9w5l4l",
      "tag_1",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "tnnd0g",
      "tag_2",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-o745fn",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799776,
    "updatedAt": 1589585799776,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "tnnd0g",
      "tag_2",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "j17rm4",
      "tag_3",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-t0c88l",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799800,
    "updatedAt": 1589585799800,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "j17rm4",
      "tag_3",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "cvl9us",
      "tag_4",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-d94xa2",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799827,
    "updatedAt": 1589585799827,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "cvl9us",
      "tag_4",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "5cohn4",
      "tag_5",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-lff0wq",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799851,
    "updatedAt": 1589585799851,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "5cohn4",
      "tag_5",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "hfjdhu",
      "tag_6",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-p8e1yv",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799881,
    "updatedAt": 1589585799881,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "hfjdhu",
      "tag_6",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "f5nw95",
      "tag_7",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-i0rzpe",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799906,
    "updatedAt": 1589585799906,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "f5nw95",
      "tag_7",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "myafcm",
      "tag_8",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-jdws15",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799929,
    "updatedAt": 1589585799929,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "myafcm",
      "tag_8",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "bggata",
      "tag_9",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-76lj42",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799955,
    "updatedAt": 1589585799955,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "bggata",
      "tag_9",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "v3i3g1",
      "tag_10",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-biq800",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585799990,
    "updatedAt": 1589585799990,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "v3i3g1",
      "tag_10",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "1uf0zg",
      "tag_11",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-2jft8k",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800019,
    "updatedAt": 1589585800019,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "1uf0zg",
      "tag_11",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "ipyet1",
      "tag_12",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-3zn93s",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800043,
    "updatedAt": 1589585800043,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "ipyet1",
      "tag_12",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "uzcde8",
      "tag_13",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-syahfk",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800066,
    "updatedAt": 1589585800066,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "uzcde8",
      "tag_13",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "mverlk",
      "tag_14",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-snxvqw",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800092,
    "updatedAt": 1589585800092,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "mverlk",
      "tag_14",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "7iwr1z",
      "tag_15",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-fr59vt",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800114,
    "updatedAt": 1589585800114,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "7iwr1z",
      "tag_15",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "nb9pf9",
      "tag_16",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-ql78vk",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800143,
    "updatedAt": 1589585800143,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "nb9pf9",
      "tag_16",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "oxl2g9",
      "tag_17",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-zduik8",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800171,
    "updatedAt": 1589585800171,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "oxl2g9",
      "tag_17",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "yhr17",
      "tag_18",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-ivm2th",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800208,
    "updatedAt": 1589585800208,
    "author": {
      "username": "authoress-8ja3wy",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "yhr17",
      "tag_18",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "v3i9hy",
      "tag_19",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-1eo9w",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585800244,
    "updatedAt": 1589585800244,
    "author": {
      "username": "author-yqr3kj",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "v3i9hy",
      "tag_19",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should list articles
```
GET /articles
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v3i9hy"
      ],
      "createdAt": 1589585800244,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1eo9w",
      "updatedAt": 1589585800244,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "yhr17"
      ],
      "createdAt": 1589585800208,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ivm2th",
      "updatedAt": 1589585800208,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "oxl2g9",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800171,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zduik8",
      "updatedAt": 1589585800171,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nb9pf9",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585800143,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ql78vk",
      "updatedAt": 1589585800143,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7iwr1z",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800114,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-fr59vt",
      "updatedAt": 1589585800114,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mverlk",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800092,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-snxvqw",
      "updatedAt": 1589585800092,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uzcde8"
      ],
      "createdAt": 1589585800066,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-syahfk",
      "updatedAt": 1589585800066,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ipyet1",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800043,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zn93s",
      "updatedAt": 1589585800043,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "1uf0zg",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800019,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2jft8k",
      "updatedAt": 1589585800019,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1",
        "v3i3g1"
      ],
      "createdAt": 1589585799990,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-biq800",
      "updatedAt": 1589585799990,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "bggata",
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799955,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-76lj42",
      "updatedAt": 1589585799955,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "myafcm",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799929,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-jdws15",
      "updatedAt": 1589585799929,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f5nw95",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799906,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-i0rzpe",
      "updatedAt": 1589585799906,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "hfjdhu",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799881,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-p8e1yv",
      "updatedAt": 1589585799881,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "5cohn4",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799851,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lff0wq",
      "updatedAt": 1589585799851,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "cvl9us",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799827,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-d94xa2",
      "updatedAt": 1589585799827,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "j17rm4",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799800,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t0c88l",
      "updatedAt": 1589585799800,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2",
        "tnnd0g"
      ],
      "createdAt": 1589585799776,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-o745fn",
      "updatedAt": 1589585799776,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "9w5l4l",
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799748,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-187oly",
      "updatedAt": 1589585799748,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kx1dys",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799705,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-obzifs",
      "updatedAt": 1589585799705,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles with tag
```
GET /articles?tag=tag_7
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "f5nw95",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799906,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-i0rzpe",
      "updatedAt": 1589585799906,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
GET /articles?tag=tag_mod_3_2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "oxl2g9",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800171,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zduik8",
      "updatedAt": 1589585800171,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mverlk",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800092,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-snxvqw",
      "updatedAt": 1589585800092,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "1uf0zg",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800019,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2jft8k",
      "updatedAt": 1589585800019,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "myafcm",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799929,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-jdws15",
      "updatedAt": 1589585799929,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "5cohn4",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799851,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lff0wq",
      "updatedAt": 1589585799851,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2",
        "tnnd0g"
      ],
      "createdAt": 1589585799776,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-o745fn",
      "updatedAt": 1589585799776,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles by author
```
GET /articles?author=authoress-8ja3wy
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "yhr17"
      ],
      "createdAt": 1589585800208,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ivm2th",
      "updatedAt": 1589585800208,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nb9pf9",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585800143,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ql78vk",
      "updatedAt": 1589585800143,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mverlk",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800092,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-snxvqw",
      "updatedAt": 1589585800092,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ipyet1",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800043,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zn93s",
      "updatedAt": 1589585800043,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1",
        "v3i3g1"
      ],
      "createdAt": 1589585799990,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-biq800",
      "updatedAt": 1589585799990,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "myafcm",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799929,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-jdws15",
      "updatedAt": 1589585799929,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "hfjdhu",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799881,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-p8e1yv",
      "updatedAt": 1589585799881,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "cvl9us",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799827,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-d94xa2",
      "updatedAt": 1589585799827,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2",
        "tnnd0g"
      ],
      "createdAt": 1589585799776,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-o745fn",
      "updatedAt": 1589585799776,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kx1dys",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799705,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-obzifs",
      "updatedAt": 1589585799705,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles favorited by user
```
GET /articles?favorited=non-author-rb756c
```
```
200 OK

{
  "articles": []
}
```
### should list articles by limit/offset
```
GET /articles?author=author-yqr3kj&limit=2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v3i9hy"
      ],
      "createdAt": 1589585800244,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1eo9w",
      "updatedAt": 1589585800244,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "oxl2g9",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800171,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zduik8",
      "updatedAt": 1589585800171,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
GET /articles?author=author-yqr3kj&limit=2&offset=2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "7iwr1z",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800114,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-fr59vt",
      "updatedAt": 1589585800114,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uzcde8"
      ],
      "createdAt": 1589585800066,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-syahfk",
      "updatedAt": 1589585800066,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles when authenticated
```
GET /articles
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v3i9hy"
      ],
      "createdAt": 1589585800244,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1eo9w",
      "updatedAt": 1589585800244,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "yhr17"
      ],
      "createdAt": 1589585800208,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ivm2th",
      "updatedAt": 1589585800208,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "oxl2g9",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800171,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zduik8",
      "updatedAt": 1589585800171,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nb9pf9",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585800143,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ql78vk",
      "updatedAt": 1589585800143,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7iwr1z",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800114,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-fr59vt",
      "updatedAt": 1589585800114,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mverlk",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800092,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-snxvqw",
      "updatedAt": 1589585800092,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uzcde8"
      ],
      "createdAt": 1589585800066,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-syahfk",
      "updatedAt": 1589585800066,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ipyet1",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800043,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zn93s",
      "updatedAt": 1589585800043,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "1uf0zg",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800019,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2jft8k",
      "updatedAt": 1589585800019,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1",
        "v3i3g1"
      ],
      "createdAt": 1589585799990,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-biq800",
      "updatedAt": 1589585799990,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "bggata",
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799955,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-76lj42",
      "updatedAt": 1589585799955,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "myafcm",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799929,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-jdws15",
      "updatedAt": 1589585799929,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f5nw95",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799906,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-i0rzpe",
      "updatedAt": 1589585799906,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "hfjdhu",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799881,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-p8e1yv",
      "updatedAt": 1589585799881,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "5cohn4",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799851,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lff0wq",
      "updatedAt": 1589585799851,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "cvl9us",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799827,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-d94xa2",
      "updatedAt": 1589585799827,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "j17rm4",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799800,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t0c88l",
      "updatedAt": 1589585799800,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2",
        "tnnd0g"
      ],
      "createdAt": 1589585799776,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-o745fn",
      "updatedAt": 1589585799776,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "9w5l4l",
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799748,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-187oly",
      "updatedAt": 1589585799748,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kx1dys",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799705,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-obzifs",
      "updatedAt": 1589585799705,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should disallow multiple of author/tag/favorited
```
GET /articles?tag=foo&author=bar
```
```
GET /articles?author=foo&favorited=bar
```
```
GET /articles?favorited=foo&tag=bar
```
## Feed
### should get feed
```
GET /articles/feed
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
200 OK

{
  "articles": []
}
```
```
POST /profiles/authoress-8ja3wy/follow

{}
```
```
200 OK

{
  "profile": {
    "username": "authoress-8ja3wy",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /articles/feed
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "yhr17"
      ],
      "createdAt": 1589585800208,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ivm2th",
      "updatedAt": 1589585800208,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nb9pf9",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585800143,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ql78vk",
      "updatedAt": 1589585800143,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mverlk",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800092,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-snxvqw",
      "updatedAt": 1589585800092,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ipyet1",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800043,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zn93s",
      "updatedAt": 1589585800043,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1",
        "v3i3g1"
      ],
      "createdAt": 1589585799990,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-biq800",
      "updatedAt": 1589585799990,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "myafcm",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799929,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-jdws15",
      "updatedAt": 1589585799929,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "hfjdhu",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799881,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-p8e1yv",
      "updatedAt": 1589585799881,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "cvl9us",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799827,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-d94xa2",
      "updatedAt": 1589585799827,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2",
        "tnnd0g"
      ],
      "createdAt": 1589585799776,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-o745fn",
      "updatedAt": 1589585799776,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kx1dys",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799705,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-obzifs",
      "updatedAt": 1589585799705,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
POST /profiles/author-yqr3kj/follow

{}
```
```
200 OK

{
  "profile": {
    "username": "author-yqr3kj",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /articles/feed
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v3i9hy"
      ],
      "createdAt": 1589585800244,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1eo9w",
      "updatedAt": 1589585800244,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "yhr17"
      ],
      "createdAt": 1589585800208,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ivm2th",
      "updatedAt": 1589585800208,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "oxl2g9",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800171,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zduik8",
      "updatedAt": 1589585800171,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nb9pf9",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585800143,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-ql78vk",
      "updatedAt": 1589585800143,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7iwr1z",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800114,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-fr59vt",
      "updatedAt": 1589585800114,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mverlk",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800092,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-snxvqw",
      "updatedAt": 1589585800092,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uzcde8"
      ],
      "createdAt": 1589585800066,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-syahfk",
      "updatedAt": 1589585800066,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ipyet1",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585800043,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zn93s",
      "updatedAt": 1589585800043,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "1uf0zg",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585800019,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2jft8k",
      "updatedAt": 1589585800019,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1",
        "v3i3g1"
      ],
      "createdAt": 1589585799990,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-biq800",
      "updatedAt": 1589585799990,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "bggata",
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799955,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-76lj42",
      "updatedAt": 1589585799955,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "myafcm",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799929,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-jdws15",
      "updatedAt": 1589585799929,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f5nw95",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799906,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-i0rzpe",
      "updatedAt": 1589585799906,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "hfjdhu",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799881,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-p8e1yv",
      "updatedAt": 1589585799881,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "5cohn4",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1589585799851,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lff0wq",
      "updatedAt": 1589585799851,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "cvl9us",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799827,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-d94xa2",
      "updatedAt": 1589585799827,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "j17rm4",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799800,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t0c88l",
      "updatedAt": 1589585799800,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2",
        "tnnd0g"
      ],
      "createdAt": 1589585799776,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-o745fn",
      "updatedAt": 1589585799776,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "9w5l4l",
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1589585799748,
      "author": {
        "username": "author-yqr3kj",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-187oly",
      "updatedAt": 1589585799748,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kx1dys",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1589585799705,
      "author": {
        "username": "authoress-8ja3wy",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-obzifs",
      "updatedAt": 1589585799705,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should disallow unauthenticated feed
```
GET /articles/feed
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
## Tags
### should get tags
```
GET /tags
```
```
200 OK

{
  "tags": [
    "7iwr1z",
    "tag_15",
    "tag_mod_2_1",
    "tag_mod_3_0",
    "oxl2g9",
    "tag_17",
    "tag_mod_3_2",
    "hfjdhu",
    "tag_6",
    "tag_mod_2_0",
    "f5nw95",
    "tag_7",
    "tag_mod_3_1",
    "tag_19",
    "v3i9hy",
    "1uf0zg",
    "tag_11",
    "kx1dys",
    "tag_0",
    "j17rm4",
    "tag_3",
    "bggata",
    "tag_9",
    "tag_18",
    "yhr17",
    "mverlk",
    "tag_14",
    "cvl9us",
    "tag_4",
    "tag_13",
    "uzcde8",
    "5cohn4",
    "tag_5",
    "tag_10",
    "v3i3g1",
    "nb9pf9",
    "tag_16",
    "ipyet1",
    "tag_12",
    "tag_a",
    "tag_b",
    "tag_2",
    "tnnd0g",
    "myafcm",
    "tag_8",
    "9w5l4l",
    "tag_1"
  ]
}
```
# Comment
```
POST /users

{
  "user": {
    "email": "author-r4003@email.com",
    "username": "author-r4003",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "author-r4003@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvci1yNDAwMyIsImlhdCI6MTU4OTU4NTgwMSwiZXhwIjoxNTg5NzU4NjAxfQ.GgQZmwDQWMHT6KrWvxMTd7JSgKjeaG1X6upVtxYvpME",
    "username": "author-r4003",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "commenter-eef0q@email.com",
    "username": "commenter-eef0q",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "commenter-eef0q@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImNvbW1lbnRlci1lZWYwcSIsImlhdCI6MTU4OTU4NTgwMSwiZXhwIjoxNTg5NzU4NjAxfQ.LxSHvTODaoyAnCSp-yBHil15883GzzGIr7XnVialhSs",
    "username": "commenter-eef0q",
    "bio": "",
    "image": ""
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body"
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-xv2qrm",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1589585801714,
    "updatedAt": 1589585801714,
    "author": {
      "username": "author-r4003",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
## Create
### should create comment
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment tgo16a"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "ad4830ef-2566-4023-8d78-c8279bbc60c8",
    "slug": "title-xv2qrm",
    "body": "test comment tgo16a",
    "createdAt": 1589585801742,
    "updatedAt": 1589585801742,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment xxve2k"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "b0074291-66a1-4582-9fb7-dbc1b3e5b1c6",
    "slug": "title-xv2qrm",
    "body": "test comment xxve2k",
    "createdAt": 1589585801770,
    "updatedAt": 1589585801770,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment 4idtrf"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "2e59ee54-7491-45c6-a07f-2741f98a3315",
    "slug": "title-xv2qrm",
    "body": "test comment 4idtrf",
    "createdAt": 1589585801799,
    "updatedAt": 1589585801799,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment dntt21"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "2f7a5483-4999-48a9-9464-86f4917a52ce",
    "slug": "title-xv2qrm",
    "body": "test comment dntt21",
    "createdAt": 1589585801832,
    "updatedAt": 1589585801832,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment i6w0mc"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "ea4b03f5-6acf-4efb-addf-f330afe426f8",
    "slug": "title-xv2qrm",
    "body": "test comment i6w0mc",
    "createdAt": 1589585801873,
    "updatedAt": 1589585801873,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment tmdfdy"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "986dd80e-fcc5-4fc8-9554-51f9650084d5",
    "slug": "title-xv2qrm",
    "body": "test comment tmdfdy",
    "createdAt": 1589585801904,
    "updatedAt": 1589585801904,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment dnggte"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "45958472-3c91-4b36-9fb1-b2315c88aeb4",
    "slug": "title-xv2qrm",
    "body": "test comment dnggte",
    "createdAt": 1589585801930,
    "updatedAt": 1589585801930,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment r29j00"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "cae27344-f662-4cfd-ba8b-24139857a248",
    "slug": "title-xv2qrm",
    "body": "test comment r29j00",
    "createdAt": 1589585801956,
    "updatedAt": 1589585801956,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment dcnn7"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "c7bd8fdb-e7f5-4250-b255-9ab05b18cbba",
    "slug": "title-xv2qrm",
    "body": "test comment dcnn7",
    "createdAt": 1589585801985,
    "updatedAt": 1589585801985,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-xv2qrm/comments

{
  "comment": {
    "body": "test comment 1es16j"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "ad423018-140a-46a3-86f4-5a5105ea6742",
    "slug": "title-xv2qrm",
    "body": "test comment 1es16j",
    "createdAt": 1589585802018,
    "updatedAt": 1589585802018,
    "author": {
      "username": "commenter-eef0q",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
### should disallow unauthenticated user
```
POST /articles/title-xv2qrm/comments

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should enforce comment body
```
POST /articles/title-xv2qrm/comments

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Comment must be specified."
    ]
  }
}
```
### should disallow non-existent article
```
POST /articles/foobar/comments

{
  "comment": {
    "body": "test comment s0l7n8"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foobar]"
    ]
  }
}
```
## Get
### should get all comments for article
```
GET /articles/title-xv2qrm/comments
```
```
200 OK

{
  "comments": [
    {
      "createdAt": 1589585801873,
      "id": "ea4b03f5-6acf-4efb-addf-f330afe426f8",
      "body": "test comment i6w0mc",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801873
    },
    {
      "createdAt": 1589585801985,
      "id": "c7bd8fdb-e7f5-4250-b255-9ab05b18cbba",
      "body": "test comment dcnn7",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801985
    },
    {
      "createdAt": 1589585801956,
      "id": "cae27344-f662-4cfd-ba8b-24139857a248",
      "body": "test comment r29j00",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801956
    },
    {
      "createdAt": 1589585801904,
      "id": "986dd80e-fcc5-4fc8-9554-51f9650084d5",
      "body": "test comment tmdfdy",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801904
    },
    {
      "createdAt": 1589585801742,
      "id": "ad4830ef-2566-4023-8d78-c8279bbc60c8",
      "body": "test comment tgo16a",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801742
    },
    {
      "createdAt": 1589585801930,
      "id": "45958472-3c91-4b36-9fb1-b2315c88aeb4",
      "body": "test comment dnggte",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801930
    },
    {
      "createdAt": 1589585801770,
      "id": "b0074291-66a1-4582-9fb7-dbc1b3e5b1c6",
      "body": "test comment xxve2k",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801770
    },
    {
      "createdAt": 1589585801799,
      "id": "2e59ee54-7491-45c6-a07f-2741f98a3315",
      "body": "test comment 4idtrf",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801799
    },
    {
      "createdAt": 1589585801832,
      "id": "2f7a5483-4999-48a9-9464-86f4917a52ce",
      "body": "test comment dntt21",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585801832
    },
    {
      "createdAt": 1589585802018,
      "id": "ad423018-140a-46a3-86f4-5a5105ea6742",
      "body": "test comment 1es16j",
      "slug": "title-xv2qrm",
      "author": {
        "username": "commenter-eef0q",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1589585802018
    }
  ]
}
```
## Delete
### should delete comment
```
DELETE /articles/title-xv2qrm/comments/ad4830ef-2566-4023-8d78-c8279bbc60c8
```
```
200 OK

{}
```
### only comment author should be able to delete comment
```
DELETE /articles/title-xv2qrm/comments/b0074291-66a1-4582-9fb7-dbc1b3e5b1c6
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only comment author can delete: [commenter-eef0q]"
    ]
  }
}
```
### should disallow unauthenticated user
```
DELETE /articles/title-xv2qrm/comments/b0074291-66a1-4582-9fb7-dbc1b3e5b1c6
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow deleting unknown comment
```
DELETE /articles/title-xv2qrm/comments/foobar_id
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Comment ID not found: [foobar_id]"
    ]
  }
}
```
# User
## Create
### should create user
```
POST /users

{
  "user": {
    "email": "user1-0.23vl0zsajgu@email.com",
    "username": "user1-0.23vl0zsajgu",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user1-0.23vl0zsajgu@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuMjN2bDB6c2FqZ3UiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.hR1-TnZCCuuba5BmHhzmKaezGtARwiU3LUO780N0g0Q",
    "username": "user1-0.23vl0zsajgu",
    "bio": "",
    "image": ""
  }
}
```
### should disallow same username
```
POST /users

{
  "user": {
    "email": "user1-0.23vl0zsajgu@email.com",
    "username": "user1-0.23vl0zsajgu",
    "password": "password"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Username already taken: [user1-0.23vl0zsajgu]"
    ]
  }
}
```
### should disallow same email
```
POST /users

{
  "user": {
    "username": "user2",
    "email": "user1-0.23vl0zsajgu@email.com",
    "password": "password"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email already taken: [user1-0.23vl0zsajgu@email.com]"
    ]
  }
}
```
### should enforce required fields
```
POST /users

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "foo": 1
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Username must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "username": 1
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "username": 1,
    "email": 2
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Password must be specified."
    ]
  }
}
```
## Login
### should login
```
POST /users/login

{
  "user": {
    "email": "user1-0.23vl0zsajgu@email.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user1-0.23vl0zsajgu@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuMjN2bDB6c2FqZ3UiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.hR1-TnZCCuuba5BmHhzmKaezGtARwiU3LUO780N0g0Q",
    "username": "user1-0.23vl0zsajgu",
    "bio": "",
    "image": ""
  }
}
```
### should disallow unknown email
```
POST /users/login

{
  "user": {
    "email": "0.c3mpam72ce6",
    "password": "somepassword"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email not found: [0.c3mpam72ce6]"
    ]
  }
}
```
### should disallow wrong password
```
POST /users/login

{
  "user": {
    "email": "user1-0.23vl0zsajgu@email.com",
    "password": "0.w5d2fx7918m"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Wrong password."
    ]
  }
}
```
### should enforce required fields
```
POST /users/login

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
```
POST /users/login

{
  "user": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email must be specified."
    ]
  }
}
```
```
POST /users/login

{
  "user": {
    "email": "someemail"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Password must be specified."
    ]
  }
}
```
## Get
### should get current user
```
GET /user
```
```
200 OK

{
  "user": {
    "email": "user1-0.23vl0zsajgu@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuMjN2bDB6c2FqZ3UiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.hR1-TnZCCuuba5BmHhzmKaezGtARwiU3LUO780N0g0Q",
    "username": "user1-0.23vl0zsajgu",
    "bio": "",
    "image": ""
  }
}
```
### should disallow bad tokens
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
## Profile
### should get profile
```
GET /profiles/user1-0.23vl0zsajgu
```
```
200 OK

{
  "profile": {
    "username": "user1-0.23vl0zsajgu",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
### should disallow unknown username
```
GET /profiles/foo_0.gxfaz0g5bto
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User not found: [foo_0.gxfaz0g5bto]"
    ]
  }
}
```
### should follow/unfollow user
```
POST /users

{
  "user": {
    "username": "followed_user",
    "email": "followed_user@mail.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "followed_user@mail.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImZvbGxvd2VkX3VzZXIiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.tY_u6T0rORUomPFgkpFdXVSTIv1f4jFQjEahc5CShgk",
    "username": "followed_user",
    "bio": "",
    "image": ""
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /profiles/followed_user
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /profiles/followed_user
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
POST /users

{
  "user": {
    "username": "user2-0.ms5ui52sik9",
    "email": "user2-0.ms5ui52sik9@mail.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user2-0.ms5ui52sik9@mail.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIyLTAubXM1dWk1MnNpazkiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.WAQ60DRxLqjUtdrr6B07TPHvjDu1khVIMpgiYc-bTPI",
    "username": "user2-0.ms5ui52sik9",
    "bio": "",
    "image": ""
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
### should disallow following with bad token
```
POST /profiles/followed_user/follow
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
## Update
### should update user
```
PUT /user

{
  "user": {
    "email": "updated-user1-0.23vl0zsajgu@email.com"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.23vl0zsajgu",
    "email": "updated-user1-0.23vl0zsajgu@email.com",
    "image": "",
    "bio": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuMjN2bDB6c2FqZ3UiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.hR1-TnZCCuuba5BmHhzmKaezGtARwiU3LUO780N0g0Q"
  }
}
```
```
PUT /user

{
  "user": {
    "password": "newpassword"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.23vl0zsajgu",
    "email": "updated-user1-0.23vl0zsajgu@email.com",
    "image": "",
    "bio": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuMjN2bDB6c2FqZ3UiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.hR1-TnZCCuuba5BmHhzmKaezGtARwiU3LUO780N0g0Q"
  }
}
```
```
PUT /user

{
  "user": {
    "bio": "newbio"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.23vl0zsajgu",
    "bio": "newbio",
    "image": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuMjN2bDB6c2FqZ3UiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.hR1-TnZCCuuba5BmHhzmKaezGtARwiU3LUO780N0g0Q"
  }
}
```
```
PUT /user

{
  "user": {
    "image": "newimage"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.23vl0zsajgu",
    "image": "newimage",
    "bio": "newbio",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuMjN2bDB6c2FqZ3UiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.hR1-TnZCCuuba5BmHhzmKaezGtARwiU3LUO780N0g0Q"
  }
}
```
### should disallow missing token/email in update
```
PUT /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
PUT /user

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
### should disallow reusing email
```
POST /users

{
  "user": {
    "email": "user2-0.u724smv9uo8@email.com",
    "username": "user2-0.u724smv9uo8",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user2-0.u724smv9uo8@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIyLTAudTcyNHNtdjl1bzgiLCJpYXQiOjE1ODk1ODU4MDIsImV4cCI6MTU4OTc1ODYwMn0.bP8B6Bs7J3sIWMNsml-VkJcruIVdNmIXVwSo7gKbzzo",
    "username": "user2-0.u724smv9uo8",
    "bio": "",
    "image": ""
  }
}
```
```
PUT /user

{
  "user": {
    "email": "user2-0.u724smv9uo8@email.com"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email already taken: [user2-0.u724smv9uo8@email.com]"
    ]
  }
}
```
# Util
## Ping
### should ping
```
GET /ping
```
```
200 OK

{
  "pong": "2020-05-15T23:36:43.011Z",
  "AWS_REGION": "us-east-1",
  "DYNAMODB_NAMESPACE": "dev"
}
```
