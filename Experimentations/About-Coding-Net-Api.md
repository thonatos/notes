github上面的api挺明确，使用也很方便，但是墙内的生活，你们懂，

所以稍微看了一下coding的代码结构，大体上就是这个样子~

	CODING_API = https://coding.net/api

### Format
	
* CODING_API/user/USER/project/PROJECT/git/blob/BRANCH/PATH
* CODING_API/user/USER/project/PROJECT/git/treeinfo/BRANCH/PATH

### Example

* /user/thonatos/project/grimrock.org/git/treeinfo/master/service
* /user/thonatos/project/grimrock.org/git/blob/master/app.js
* /user/thonatos/project/grimrock.org/git/blob/master/service/queryService.js

#### Tree

```
{
    "code": 0,
    "data": {
        "infos": [
            {
                "lastCommitMessage": "Init\n",
                "lastCommitDate": 1418543563000,
                "lastCommitId": "4cbeefe7eec0a03a17d04afdfe334ef6028fa375",
                "lastCommitter": {
                    "name": "Thonatos.Yang",
                    "email": "thonatos.yang@gmail.com",
                    "avatar": "https://www.gravatar.com/avatar/281ce5fb158941764bc390600be91610?s=200&d=mm",
                    "link": "mailto:thonatos.yang@gmail.com"
                },
                "mode": "file",
                "path": "service/queryService.js",
                "name": "queryService.js"
            },
            {
                "lastCommitMessage": "Init\n",
                "lastCommitDate": 1418543563000,
                "lastCommitId": "4cbeefe7eec0a03a17d04afdfe334ef6028fa375",
                "lastCommitter": {
                    "name": "Thonatos.Yang",
                    "email": "thonatos.yang@gmail.com",
                    "avatar": "https://www.gravatar.com/avatar/281ce5fb158941764bc390600be91610?s=200&d=mm",
                    "link": "mailto:thonatos.yang@gmail.com"
                },
                "mode": "file",
                "path": "service/renderService.js",
                "name": "renderService.js"
            }
        ]
    }
}
```

#### File

```
{
    "code": 0,
    "data": {
        "ref": "master",
        "can_edit": true,
        "isHead": true,
        "file": {
            "data": "此处省略万字",
            "lang": "javascript",
            "size": 1252,
            "previewed": false,
            "lastCommitMessage": "Init\n",
            "lastCommitDate": 1418543563000,
            "lastCommitId": "4cbeefe7eec0a03a17d04afdfe334ef6028fa375",
            "lastCommitter": {
                "name": "Thonatos.Yang",
                "email": "thonatos.yang@gmail.com",
                "avatar": "https://www.gravatar.com/avatar/281ce5fb158941764bc390600be91610?s=200&d=mm",
                "link": "mailto:thonatos.yang@gmail.com"
            },
            "mode": "file",
            "path": "service/queryService.js",
            "name": "service/queryService.js"
        }
    }
}
```