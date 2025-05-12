
# 📦 MongoDB Aggregation Query Manual
**Project:** Real-Time Social Media Feed Analysis (NoSQL, MongoDB)

This document contains 19 written MongoDB aggregation queries categorized by topic. These are optimized for a social media analytics dataset with `users` and `posts` collections in a MongoDB database.

---

## 👤 USER BEHAVIOR ANALYSIS

### 1. Top 10 Most Followed Users
```js
db.users.aggregate([
  { $project: { username: 1, followers_count: { $size: "$followers" } } },
  { $sort: { followers_count: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f85b9"
    },
    "username": "Georgiana.Predovic66",
    "followers_count": 31
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8548"
    },
    "username": "Travon62",
    "followers_count": 31
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f85d3"
    },
    "username": "Kelvin75",
    "followers_count": 29
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8748"
    },
    "username": "Noah_Lowe",
    "followers_count": 29
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8582"
    },
    "username": "Jermain.Bailey93",
    "followers_count": 29
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f86b6"
    },
    "username": "Earlene32",
    "followers_count": 28
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f87b4"
    },
    "username": "Zachery.Miller15",
    "followers_count": 28
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8724"
    },
    "username": "Geoffrey_Predovic72",
    "followers_count": 28
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8612"
    },
    "username": "Fanny10",
    "followers_count": 28
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8595"
    },
    "username": "Casimer.Leuschke34",
    "followers_count": 28
  }
]
```

### 2. Most Active Users by Post Count
```js
db.posts.aggregate([
  { $group: { _id: "$author_id", post_count: { $sum: 1 } } },
  { $sort: { post_count: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": "user_11",
    "post_count": 10
  },
  {
    "_id": "user_121",
    "post_count": 10
  },
  {
    "_id": "user_424",
    "post_count": 9
  },
  {
    "_id": "user_942",
    "post_count": 9
  },
  {
    "_id": "user_478",
    "post_count": 9
  },
  {
    "_id": "user_398",
    "post_count": 9
  },
  {
    "_id": "user_937",
    "post_count": 9
  },
  {
    "_id": "user_85",
    "post_count": 9
  },
  {
    "_id": "user_664",
    "post_count": 9
  },
  {
    "_id": "user_828",
    "post_count": 9
  }
]
```

### 3. Users with Highest Mutual Follows
```js
db.users.aggregate([
  {
    $project: {
      username: 1,
      mutual_count: {
        $size: { $setIntersection: ["$followers", "$following"] }
      }
    }
  },
  { $sort: { mutual_count: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8554"
    },
    "username": "Arely83",
    "mutual_count": 4
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f85da"
    },
    "username": "Dedrick_Murphy",
    "mutual_count": 4
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f87ee"
    },
    "username": "Lilyan.Beahan",
    "mutual_count": 3
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8780"
    },
    "username": "Maya93",
    "mutual_count": 3
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f866e"
    },
    "username": "Iliana95",
    "mutual_count": 3
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8465"
    },
    "username": "Ronny47",
    "mutual_count": 3
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f845b"
    },
    "username": "Asa_Block4",
    "mutual_count": 2
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84c3"
    },
    "username": "Elijah.OHara",
    "mutual_count": 2
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f849c"
    },
    "username": "Domenica61",
    "mutual_count": 2
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84e1"
    },
    "username": "Ansley_Larkin",
    "mutual_count": 2
  }
]
```

### 4. Users with One-Way Follows Only
```js
db.users.aggregate([
  {
    $project: {
      id: 1,
      one_way_follows: { $setDifference: ["$following", "$followers"] }
    }
  },
  {
    $project: {
      id: 1,
      one_way_count: { $size: "$one_way_follows" }
    }
  },
  { $sort: { one_way_count: -1 } }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84a8"
    },
    "id": "user_97",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8662"
    },
    "id": "user_539",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f85dd"
    },
    "id": "user_406",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f848f"
    },
    "id": "user_72",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f87c2"
    },
    "id": "user_891",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8735"
    },
    "id": "user_750",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f85fa"
    },
    "id": "user_435",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8663"
    },
    "id": "user_540",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8731"
    },
    "id": "user_746",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8605"
    },
    "id": "user_446",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8503"
    },
    "id": "user_188",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f85fd"
    },
    "id": "user_438",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f86b4"
    },
    "id": "user_621",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f85ee"
    },
    "id": "user_423",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f87eb"
    },
    "id": "user_932",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f87e5"
    },
    "id": "user_926",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f87d1"
    },
    "id": "user_906",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84d4"
    },
    "id": "user_141",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f86a2"
    },
    "id": "user_603",
    "one_way_count": 30
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f873e"
    },
    "id": "user_759",
    "one_way_count": 30
  }
]
```

---

## 💬 ENGAGEMENT & VIRALITY

### 5. Top 5 Viral Posts by Engagement Score (Likes + 2x Comments)
```js
db.posts.aggregate([
  {
    $addFields: {
      engagement_score: {
        $add: [{ $size: "$likes" }, { $multiply: [{ $size: "$comments" }, 2] }]
      }
    }
  },
  { $sort: { engagement_score: -1 } },
  { $limit: 5 },
  { $project: { text: 1, engagement_score: 1 } }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8c00"
    },
    "text": "Craving days like this again 💫",
    "engagement_score": 40
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8b87"
    },
    "text": "Smiles, sunlight, and good energy ✨",
    "engagement_score": 40
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8f21"
    },
    "text": "Sunsets like these remind me to slow down 🌅",
    "engagement_score": 40
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8ed5"
    },
    "text": "This is what balance looks like 🧘‍♀️",
    "engagement_score": 40
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8a6c"
    },
    "text": "What a peaceful morning walk today 🌤️",
    "engagement_score": 40
  }
]
```

### 6. Posts with Zero Engagement (No Likes or Comments)
```js
db.posts.aggregate([
  {
    $addFields: {
      likes_count: { $size: "$likes" },
      comments_count: { $size: "$comments" }
    }
  },
  {
    $match: {
      likes_count: 0,
      comments_count: 0
    }
  },
  { $project: { id: 1, text: 1, timestamp: 1 } }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f88ef"
    },
    "id": "post_191",
    "text": "Couldn’t resist snapping this shot!",
    "timestamp": "2025-05-08T02:56:02.633Z"
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f891a"
    },
    "id": "post_234",
    "text": "Craving days like this again 💫",
    "timestamp": "2025-05-07T11:50:45.012Z"
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f892e"
    },
    "id": "post_254",
    "text": "This view took my breath away. Nature wins every time 🌿",
    "timestamp": "2025-05-07T14:59:36.203Z"
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8a25"
    },
    "id": "post_501",
    "text": "This one’s for the memory book 💭",
    "timestamp": "2025-05-07T21:31:06.976Z"
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8ac4"
    },
    "id": "post_660",
    "text": "Nothing like fresh air and good company.",
    "timestamp": "2025-05-08T03:09:42.285Z"
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8b83"
    },
    "id": "post_851",
    "text": "Nothing like fresh air and good company.",
    "timestamp": "2025-05-07T17:11:25.929Z"
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8c92"
    },
    "id": "post_1122",
    "text": "Coffee in one hand, confidence in the other ☕💪",
    "timestamp": "2025-05-07T12:12:40.681Z"
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8db8"
    },
    "id": "post_1416",
    "text": "A picture’s worth a thousand feelings.",
    "timestamp": "2025-05-07T09:42:53.769Z"
  },
  {
    "_id": {
      "$oid": "681f3a99f1e469bde09f905b"
    },
    "id": "post_2091",
    "text": "This one’s for the memory book 💭",
    "timestamp": "2025-05-08T02:01:25.270Z"
  },
  {
    "_id": {
      "$oid": "681f3a99f1e469bde09f9104"
    },
    "id": "post_2260",
    "text": "A picture’s worth a thousand feelings.",
    "timestamp": "2025-05-07T13:49:46.556Z"
  },
  {
    "_id": {
      "$oid": "681f3a99f1e469bde09f926f"
    },
    "id": "post_2623",
    "text": "What a peaceful morning walk today 🌤️",
    "timestamp": "2025-05-07T04:28:49.632Z"
  },
  {
    "_id": {
      "$oid": "681f3a99f1e469bde09f9367"
    },
    "id": "post_2871",
    "text": "Can't believe how good this turned out!",
    "timestamp": "2025-05-08T01:28:03.782Z"
  },
  {
    "_id": {
      "$oid": "681f3a99f1e469bde09f937c"
    },
    "id": "post_2892",
    "text": "What a peaceful morning walk today 🌤️",
    "timestamp": "2025-05-07T04:23:08.150Z"
  }
]
```

### 7. Posts with Highest Like-to-Comment Ratio
```js
db.posts.aggregate([
  {
    $addFields: {
      likes_count: { $size: "$likes" },
      comments_count: { $size: "$comments" },
      like_comment_ratio: {
        $cond: [
          { $eq: [{ $size: "$comments" }, 0] },
          null,
          { $divide: [{ $size: "$likes" }, { $size: "$comments" }] }
        ]
      }
    }
  },
  { $match: { like_comment_ratio: { $ne: null } } },
  { $sort: { like_comment_ratio: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f896a"
    },
    "id": "post_314",
    "author_id": "user_992",
    "text": "Coffee in one hand, confidence in the other ☕💪",
    "media_url": "https://loremflickr.com/3336/2288?lock=6201838358464461",
    "hashtags": [
      "sunt"
    ],
    "location": "Lake Rosamond",
    "likes": [
      "user_645",
      "user_789",
      "user_654",
      "user_187",
      "user_618",
      "user_678",
      "user_832",
      "user_214",
      "user_313",
      "user_244",
      "user_169",
      "user_899",
      "user_326",
      "user_348",
      "user_656",
      "user_772",
      "user_574",
      "user_37",
      "user_697",
      "user_759"
    ],
    "comments": [
      {
        "user_id": "user_246",
        "text": "This is why I follow you.",
        "timestamp": "2025-05-07T12:53:26.069Z"
      }
    ],
    "timestamp": "2025-05-07T13:21:26.909Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8ac9"
    },
    "id": "post_665",
    "author_id": "user_985",
    "text": "Simple moments are the most special.",
    "media_url": "https://picsum.photos/seed/4y6Q31TB/3291/2059",
    "hashtags": [
      "ascit",
      "unus"
    ],
    "location": "South Evangelinestead",
    "likes": [
      "user_997",
      "user_646",
      "user_327",
      "user_890",
      "user_21",
      "user_539",
      "user_687",
      "user_17",
      "user_80",
      "user_873",
      "user_946",
      "user_678",
      "user_25",
      "user_542",
      "user_228",
      "user_249",
      "user_589",
      "user_925",
      "user_674",
      "user_438"
    ],
    "comments": [
      {
        "user_id": "user_34",
        "text": "The mood is unreal.",
        "timestamp": "2025-05-08T02:23:10.831Z"
      }
    ],
    "timestamp": "2025-05-07T07:33:34.269Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a99f1e469bde09f91dc"
    },
    "id": "post_2476",
    "author_id": "user_440",
    "text": "Couldn’t resist snapping this shot!",
    "media_url": "https://picsum.photos/seed/ovA46jaEnJ/1805/52",
    "hashtags": [
      "cognomen",
      "aeternus"
    ],
    "location": "Port Hollistown",
    "likes": [
      "user_986",
      "user_517",
      "user_725",
      "user_324",
      "user_931",
      "user_716",
      "user_559",
      "user_719",
      "user_318",
      "user_842",
      "user_651",
      "user_870",
      "user_141",
      "user_453",
      "user_573",
      "user_116",
      "user_619",
      "user_86",
      "user_12",
      "user_250"
    ],
    "comments": [
      {
        "user_id": "user_36",
        "text": "This post gave me chills.",
        "timestamp": "2025-05-07T10:40:33.163Z"
      }
    ],
    "timestamp": "2025-05-07T03:31:07.489Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f89e0"
    },
    "id": "post_432",
    "author_id": "user_896",
    "text": "Smiles, sunlight, and good energy ✨",
    "media_url": "https://loremflickr.com/1527/3969?lock=5167456721742700",
    "hashtags": [
      "suggero",
      "solvo",
      "creta"
    ],
    "location": "Celineshire",
    "likes": [
      "user_710",
      "user_93",
      "user_705",
      "user_592",
      "user_630",
      "user_858",
      "user_455",
      "user_817",
      "user_310",
      "user_573",
      "user_727",
      "user_585",
      "user_498",
      "user_874",
      "user_50",
      "user_359",
      "user_287",
      "user_931",
      "user_944",
      "user_695"
    ],
    "comments": [
      {
        "user_id": "user_365",
        "text": "Pure magic!",
        "timestamp": "2025-05-07T12:32:23.538Z"
      }
    ],
    "timestamp": "2025-05-07T16:14:47.377Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8c32"
    },
    "id": "post_1026",
    "author_id": "user_488",
    "text": "Rainy day reads and cozy vibes 📚☔",
    "media_url": "https://picsum.photos/seed/edd9iMSo69/616/1337",
    "hashtags": [
      "itaque",
      "ut",
      "acerbitas"
    ],
    "location": "Deloresboro",
    "likes": [
      "user_918",
      "user_495",
      "user_981",
      "user_527",
      "user_335",
      "user_764",
      "user_84",
      "user_364",
      "user_823",
      "user_21",
      "user_649",
      "user_933",
      "user_780",
      "user_705",
      "user_517",
      "user_396",
      "user_512",
      "user_437",
      "user_45",
      "user_278"
    ],
    "comments": [
      {
        "user_id": "user_25",
        "text": "Feels like a dream 🌙",
        "timestamp": "2025-05-07T08:24:37.449Z"
      }
    ],
    "timestamp": "2025-05-07T18:32:33.860Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8a16"
    },
    "id": "post_486",
    "author_id": "user_128",
    "text": "Just me, my camera, and this magical place 📸",
    "media_url": "https://loremflickr.com/3610/2944?lock=1872680839035941",
    "hashtags": [
      "trado",
      "arcus"
    ],
    "location": "Provo",
    "likes": [
      "user_235",
      "user_188",
      "user_344",
      "user_169",
      "user_472",
      "user_232",
      "user_431",
      "user_346",
      "user_896",
      "user_375",
      "user_844",
      "user_146",
      "user_240",
      "user_769",
      "user_726",
      "user_435",
      "user_579",
      "user_485",
      "user_900",
      "user_979"
    ],
    "comments": [
      {
        "user_id": "user_833",
        "text": "Total perfection.",
        "timestamp": "2025-05-07T18:02:47.804Z"
      }
    ],
    "timestamp": "2025-05-07T15:43:17.999Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8d83"
    },
    "id": "post_1363",
    "author_id": "user_524",
    "text": "Rainy day reads and cozy vibes 📚☔",
    "media_url": "https://loremflickr.com/192/3512?lock=2310099383613479",
    "hashtags": [
      "alienus",
      "sollers",
      "somnus"
    ],
    "location": "Lavernafort",
    "likes": [
      "user_391",
      "user_933",
      "user_47",
      "user_1000",
      "user_931",
      "user_534",
      "user_844",
      "user_654",
      "user_700",
      "user_351",
      "user_514",
      "user_916",
      "user_49",
      "user_765",
      "user_616",
      "user_990",
      "user_307",
      "user_424",
      "user_258",
      "user_764"
    ],
    "comments": [
      {
        "user_id": "user_802",
        "text": "My favorite post today!",
        "timestamp": "2025-05-07T17:19:53.251Z"
      }
    ],
    "timestamp": "2025-05-07T10:13:50.457Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8a71"
    },
    "id": "post_577",
    "author_id": "user_445",
    "text": "Just me, my camera, and this magical place 📸",
    "media_url": "https://loremflickr.com/1554/2893?lock=293451774565586",
    "hashtags": [
      "vitiosus",
      "hic"
    ],
    "location": "Darionboro",
    "likes": [
      "user_315",
      "user_193",
      "user_846",
      "user_401",
      "user_579",
      "user_613",
      "user_475",
      "user_854",
      "user_546",
      "user_464",
      "user_790",
      "user_469",
      "user_427",
      "user_280",
      "user_37",
      "user_621",
      "user_274",
      "user_168",
      "user_833",
      "user_767"
    ],
    "comments": [
      {
        "user_id": "user_396",
        "text": "What camera did you use?",
        "timestamp": "2025-05-07T20:37:27.696Z"
      }
    ],
    "timestamp": "2025-05-08T02:55:23.125Z",
    "likes_count": 20,
    "comments_count": 1,
    "like_comment_ratio": 20
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8b34"
    },
    "id": "post_772",
    "author_id": "user_626",
    "text": "Feeling grateful for the little things.",
    "media_url": "https://picsum.photos/seed/aaD80R8c/1669/3275",
    "hashtags": [
      "admiratio"
    ],
    "location": "Clemmiehaven",
    "likes": [
      "user_313",
      "user_188",
      "user_591",
      "user_296",
      "user_96",
      "user_891",
      "user_410",
      "user_612",
      "user_396",
      "user_330",
      "user_651",
      "user_71",
      "user_804",
      "user_742",
      "user_242",
      "user_940",
      "user_129",
      "user_500",
      "user_511"
    ],
    "comments": [
      {
        "user_id": "user_983",
        "text": "Art in its purest form.",
        "timestamp": "2025-05-07T08:17:52.510Z"
      }
    ],
    "timestamp": "2025-05-07T18:27:25.141Z",
    "likes_count": 19,
    "comments_count": 1,
    "like_comment_ratio": 19
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f888f"
    },
    "id": "post_95",
    "author_id": "user_997",
    "text": "This view took my breath away. Nature wins every time 🌿",
    "media_url": "https://loremflickr.com/1936/3082?lock=4636150345914573",
    "hashtags": [
      "denego"
    ],
    "location": "Lehnerville",
    "likes": [
      "user_25",
      "user_26",
      "user_386",
      "user_540",
      "user_136",
      "user_271",
      "user_784",
      "user_17",
      "user_343",
      "user_576",
      "user_2",
      "user_893",
      "user_664",
      "user_516",
      "user_362",
      "user_797",
      "user_767",
      "user_615",
      "user_113"
    ],
    "comments": [
      {
        "user_id": "user_762",
        "text": "What a view!",
        "timestamp": "2025-05-07T14:17:17.321Z"
      }
    ],
    "timestamp": "2025-05-08T00:37:24.192Z",
    "likes_count": 19,
    "comments_count": 1,
    "like_comment_ratio": 19
  }
]
```

---

## 🏷️ HASHTAG ANALYSIS

### 8. Top 10 Hashtags Used
```js
db.posts.aggregate([
  { $unwind: "$hashtags" },
  { $group: { _id: "$hashtags", count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": "amor",
    "count": 19
  },
  {
    "_id": "consuasor",
    "count": 15
  },
  {
    "_id": "videlicet",
    "count": 14
  },
  {
    "_id": "aetas",
    "count": 14
  },
  {
    "_id": "ver",
    "count": 14
  },
  {
    "_id": "confugo",
    "count": 13
  },
  {
    "_id": "ascit",
    "count": 13
  },
  {
    "_id": "crinis",
    "count": 13
  },
  {
    "_id": "ultra",
    "count": 13
  },
  {
    "_id": "cruciamentum",
    "count": 13
  }
]
```

### 9. Average Number of Hashtags Per Post
```js
db.posts.aggregate([
  { $project: { hashtags_count: { $size: "$hashtags" } } },
  { $group: { _id: null, avg_hashtags: { $avg: "$hashtags_count" } } }
])
```
**Result:**
```js
[
  {
    "_id": null,
    "avg_hashtags": 2.012
  }
]
```

---

## 🕒 TIME-BASED ANALYTICS

### 10. Number of Posts Per Hour
```js
db.posts.aggregate([
  { $addFields: { hour: { $hour: { $toDate: "$timestamp" } } } },
  { $group: { _id: "$hour", count: { $sum: 1 } } },
  { $sort: { _id: 1 } }
])
```
**Result:**
```js
[
  {
    "_id": 0,
    "count": 132
  },
  {
    "_id": 1,
    "count": 121
  },
  {
    "_id": 2,
    "count": 134
  },
  {
    "_id": 3,
    "count": 129
  },
  {
    "_id": 4,
    "count": 122
  },
  {
    "_id": 5,
    "count": 131
  },
  {
    "_id": 6,
    "count": 118
  },
  {
    "_id": 7,
    "count": 118
  },
  {
    "_id": 8,
    "count": 117
  },
  {
    "_id": 9,
    "count": 107
  },
  {
    "_id": 10,
    "count": 130
  },
  {
    "_id": 11,
    "count": 127
  },
  {
    "_id": 12,
    "count": 134
  },
  {
    "_id": 13,
    "count": 119
  },
  {
    "_id": 14,
    "count": 144
  },
  {
    "_id": 15,
    "count": 136
  },
  {
    "_id": 16,
    "count": 121
  },
  {
    "_id": 17,
    "count": 126
  },
  {
    "_id": 18,
    "count": 121
  },
  {
    "_id": 19,
    "count": 132
  }
]
```

### 11. Average Engagement by Hour
```js
db.posts.aggregate([
  {
    $addFields: {
      hour: { $hour: { $toDate: "$timestamp" } },
      engagement_score: {
        $add: [{ $size: "$likes" }, { $multiply: [{ $size: "$comments" }, 2] }]
      }
    }
  },
  {
    $group: {
      _id: "$hour",
      avg_engagement: { $avg: "$engagement_score" }
    }
  },
  { $sort: { _id: 1 } }
])
```
**Result:**
```js
[
  {
    "_id": 0,
    "avg_engagement": 19.348484848484848
  },
  {
    "_id": 1,
    "avg_engagement": 18.75206611570248
  },
  {
    "_id": 2,
    "avg_engagement": 19.44776119402985
  },
  {
    "_id": 3,
    "avg_engagement": 19.131782945736433
  },
  {
    "_id": 4,
    "avg_engagement": 19.770491803278688
  },
  {
    "_id": 5,
    "avg_engagement": 18.244274809160306
  },
  {
    "_id": 6,
    "avg_engagement": 20.52542372881356
  },
  {
    "_id": 7,
    "avg_engagement": 21.703389830508474
  },
  {
    "_id": 8,
    "avg_engagement": 20.196581196581196
  },
  {
    "_id": 9,
    "avg_engagement": 19.822429906542055
  },
  {
    "_id": 10,
    "avg_engagement": 19.123076923076923
  },
  {
    "_id": 11,
    "avg_engagement": 20.11023622047244
  },
  {
    "_id": 12,
    "avg_engagement": 20.380597014925375
  },
  {
    "_id": 13,
    "avg_engagement": 19.34453781512605
  },
  {
    "_id": 14,
    "avg_engagement": 21.04861111111111
  },
  {
    "_id": 15,
    "avg_engagement": 20.389705882352942
  },
  {
    "_id": 16,
    "avg_engagement": 21.694214876033058
  },
  {
    "_id": 17,
    "avg_engagement": 19.19047619047619
  },
  {
    "_id": 18,
    "avg_engagement": 19.231404958677686
  },
  {
    "_id": 19,
    "avg_engagement": 20.856060606060606
  }
]
```
### 12. Average Engagement by Day of the Week
```js
db.posts.aggregate([
  {
    $addFields: {
      day: { $dayOfWeek: { $toDate: "$timestamp" } },
      engagement_score: {
        $add: [{ $size: "$likes" }, { $multiply: [{ $size: "$comments" }, 2] }]
      }
    }
  },
  {
    $group: {
      _id: "$day",
      avg_engagement: { $avg: "$engagement_score" }
    }
  },
  { $sort: { _id: 1 } }
])
```
**Result:**
```js
[
  {
    "_id": 4,
    "avg_engagement": 20.093896713615024
  },
  {
    "_id": 5,
    "avg_engagement": 19.105855855855857
  }
]
```

---

## 💬 COMMENT BEHAVIOR

### 13. Top 10 Commenters by Volume
```js
db.posts.aggregate([
  { $unwind: "$comments" },
  { $group: { _id: "$comments.user_id", comment_count: { $sum: 1 } } },
  { $sort: { comment_count: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": "user_587",
    "comment_count": 29
  },
  {
    "_id": "user_614",
    "comment_count": 28
  },
  {
    "_id": "user_263",
    "comment_count": 27
  },
  {
    "_id": "user_327",
    "comment_count": 26
  },
  {
    "_id": "user_522",
    "comment_count": 26
  },
  {
    "_id": "user_357",
    "comment_count": 26
  },
  {
    "_id": "user_998",
    "comment_count": 25
  },
  {
    "_id": "user_25",
    "comment_count": 25
  },
  {
    "_id": "user_556",
    "comment_count": 25
  },
  {
    "_id": "user_518",
    "comment_count": 25
  }
]
```

### 14. Total Comments Per Post
```js
db.posts.aggregate([
  { $project: { id: 1, comments_count: { $size: "$comments" } } },
  { $sort: { comments_count: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8847"
    },
    "id": "post_23",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8883"
    },
    "id": "post_83",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8899"
    },
    "id": "post_105",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8838"
    },
    "id": "post_8",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8835"
    },
    "id": "post_5",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8845"
    },
    "id": "post_21",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f88a9"
    },
    "id": "post_121",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f88a2"
    },
    "id": "post_114",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f883e"
    },
    "id": "post_14",
    "comments_count": 10
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8841"
    },
    "id": "post_17",
    "comments_count": 10
  }
]
```

---

## 📈 USER-LEVEL METRICS

### 15. Users with Highest Average Post Engagement
```js
db.posts.aggregate([
  {
    $addFields: {
      engagement: {
        $add: [{ $size: "$likes" }, { $multiply: [{ $size: "$comments" }, 2] }]
      }
    }
  },
  {
    $group: {
      _id: "$author_id",
      avg_engagement: { $avg: "$engagement" },
      post_count: { $sum: 1 }
    }
  },
  { $sort: { avg_engagement: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": "user_542",
    "avg_engagement": 40,
    "post_count": 1
  },
  {
    "_id": "user_119",
    "avg_engagement": 39,
    "post_count": 1
  },
  {
    "_id": "user_628",
    "avg_engagement": 38,
    "post_count": 2
  },
  {
    "_id": "user_961",
    "avg_engagement": 38,
    "post_count": 1
  },
  {
    "_id": "user_597",
    "avg_engagement": 38,
    "post_count": 1
  },
  {
    "_id": "user_537",
    "avg_engagement": 38,
    "post_count": 1
  },
  {
    "_id": "user_405",
    "avg_engagement": 38,
    "post_count": 1
  },
  {
    "_id": "user_484",
    "avg_engagement": 38,
    "post_count": 1
  },
  {
    "_id": "user_715",
    "avg_engagement": 36,
    "post_count": 1
  },
  {
    "_id": "user_711",
    "avg_engagement": 36,
    "post_count": 1
  }
]
```

### 16. Highest Word Count Posts
```js
db.posts.aggregate([
  {
    $project: {
      id: 1,
      text: 1,
      word_count: { $size: { $split: ["$text", " "] } }
    }
  },
  { $sort: { word_count: -1 } },
  { $limit: 5 }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8868"
    },
    "id": "post_56",
    "text": "This view took my breath away. Nature wins every time 🌿",
    "word_count": 11
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8862"
    },
    "id": "post_50",
    "text": "This view took my breath away. Nature wins every time 🌿",
    "word_count": 11
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f888e"
    },
    "id": "post_94",
    "text": "This view took my breath away. Nature wins every time 🌿",
    "word_count": 11
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f886c"
    },
    "id": "post_60",
    "text": "This view took my breath away. Nature wins every time 🌿",
    "word_count": 11
  },
  {
    "_id": {
      "$oid": "681f3a98f1e469bde09f8840"
    },
    "id": "post_16",
    "text": "This view took my breath away. Nature wins every time 🌿",
    "word_count": 11
  }
]
```

---

## 🧠 ADVANCED & CROSS-JOINED

### 17. Hashtags Appearing Only in Viral Posts
```js
db.posts.aggregate([
  {
    $addFields: {
      engagement_score: {
        $add: [{ $size: "$likes" }, { $multiply: [{ $size: "$comments" }, 2] }]
      }
    }
  },
  { $match: { engagement_score: { $gte: 20 } } },
  { $unwind: "$hashtags" },
  {
    $group: {
      _id: "$hashtags",
      viral_count: { $sum: 1 }
    }
  },
  { $sort: { viral_count: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": "tolero",
    "viral_count": 10
  },
  {
    "_id": "trado",
    "viral_count": 9
  },
  {
    "_id": "amor",
    "viral_count": 9
  },
  {
    "_id": "soluta",
    "viral_count": 9
  },
  {
    "_id": "ver",
    "viral_count": 8
  },
  {
    "_id": "ultra",
    "viral_count": 8
  },
  {
    "_id": "voco",
    "viral_count": 8
  },
  {
    "_id": "tepidus",
    "viral_count": 8
  },
  {
    "_id": "videlicet",
    "viral_count": 8
  },
  {
    "_id": "et",
    "viral_count": 8
  }
]
```

### 18. Engagement-to-Follower Efficiency (User Score)
```js
db.posts.aggregate([
  {
    $addFields: {
      engagement: {
        $add: [{ $size: "$likes" }, { $multiply: [{ $size: "$comments" }, 2] }]
      }
    }
  },
  {
    $group: {
      _id: "$author_id",
      total_engagement: { $sum: "$engagement" }
    }
  },
  {
    $lookup: {
      from: "users",
      localField: "_id",
      foreignField: "id",
      as: "user_info"
    }
  },
  { $unwind: "$user_info" },
  {
    $project: {
      total_engagement: 1,
      followers: { $size: "$user_info.followers" },
      efficiency: {
        $cond: [
          { $eq: [{ $size: "$user_info.followers" }, 0] },
          0,
          { $divide: ["$total_engagement", { $size: "$user_info.followers" }] }
        ]
      }
    }
  },
  { $sort: { efficiency: -1 } },
  { $limit: 10 }
])
```
**Result:**
```js
[
  {
    "_id": "user_937",
    "total_engagement": 214,
    "followers": 10,
    "efficiency": 21.4
  },
  {
    "_id": "user_398",
    "total_engagement": 221,
    "followers": 11,
    "efficiency": 20.09090909090909
  },
  {
    "_id": "user_569",
    "total_engagement": 76,
    "followers": 4,
    "efficiency": 19
  },
  {
    "_id": "user_478",
    "total_engagement": 210,
    "followers": 12,
    "efficiency": 17.5
  },
  {
    "_id": "user_664",
    "total_engagement": 190,
    "followers": 11,
    "efficiency": 17.272727272727273
  },
  {
    "_id": "user_670",
    "total_engagement": 168,
    "followers": 10,
    "efficiency": 16.8
  },
  {
    "_id": "user_573",
    "total_engagement": 83,
    "followers": 5,
    "efficiency": 16.6
  },
  {
    "_id": "user_85",
    "total_engagement": 242,
    "followers": 15,
    "efficiency": 16.133333333333333
  },
  {
    "_id": "user_424",
    "total_engagement": 236,
    "followers": 16,
    "efficiency": 14.75
  },
  {
    "_id": "user_818",
    "total_engagement": 146,
    "followers": 10,
    "efficiency": 14.6
  }
]
```

---

### 19. Inactive Users (No Posts)
```js
db.users.aggregate([
  {
    $lookup: {
      from: "posts",
      localField: "id",
      foreignField: "author_id",
      as: "user_posts"
    }
  },
  {
    $match: {
      user_posts: { $eq: [] }
    }
  },
  {
    $project: {
      id: 1,
      username: 1
    }
  }
])
```
**Result:**
```js
[
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8458"
    },
    "id": "user_17",
    "username": "Evans.Bernier58"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f846b"
    },
    "id": "user_36",
    "username": "Herta27"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84a7"
    },
    "id": "user_96",
    "username": "Samara98"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84ab"
    },
    "id": "user_100",
    "username": "Roscoe33"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84af"
    },
    "id": "user_104",
    "username": "Vince_Smith14"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84c2"
    },
    "id": "user_123",
    "username": "Jadyn.Douglas58"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84d1"
    },
    "id": "user_138",
    "username": "Alan_Lebsack97"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84d4"
    },
    "id": "user_141",
    "username": "Bailey.Braun"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84ea"
    },
    "id": "user_163",
    "username": "Bernadine.Konopelski69"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84f3"
    },
    "id": "user_172",
    "username": "Janick_Collins"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84fa"
    },
    "id": "user_179",
    "username": "Ellen99"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f84fd"
    },
    "id": "user_182",
    "username": "Earlene.Prosacco-Kuvalis58"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8511"
    },
    "id": "user_202",
    "username": "Fatima_Fadel42"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f852f"
    },
    "id": "user_232",
    "username": "Porter30"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8535"
    },
    "id": "user_238",
    "username": "Brandi_Pacocha"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8536"
    },
    "id": "user_239",
    "username": "Giovanny48"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8537"
    },
    "id": "user_240",
    "username": "Moses38"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8546"
    },
    "id": "user_255",
    "username": "Gracie87"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8566"
    },
    "id": "user_287",
    "username": "Demarcus_Tillman"
  },
  {
    "_id": {
      "$oid": "681f3a8cf1e469bde09f8569"
    },
    "id": "user_290",
    "username": "Ashly.Ziemann"
  }
]
```
---