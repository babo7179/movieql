# movieql
Movie API Width Graphql

# graphQL 이란
페이스북에서 만든 어플리케이션 쿼리언어


#2 problems solved by GraphQL

보통 프로젝트를 진행할때 백엔드 개발자들이 구축한 RestFull API서버에 요청을 통해 데이터를 받음 이때 over-fetching,under-fetching 두가지 문제가 발생하는데 이것을 GraphQL로 해결할수 있음

over-fetching - 예를들어 모든 유저들 이름을 웹사이트에 보여주고 싶다고 하자 GET을 쓸건데

/user/1/GET


users에서 너한테 사용자 명만 주는게 아니라, 성, 프로필사진, 성별등 불필요한 모든 유저들의 정보까지 받아오게 될것이다. 너는 오직 사용자명만 필요할 뿐인데 사용도 안할 정보를 전달받아서 쓸데없는 영역을 보게 만들고 고객들이 우리앱에서 필요도 없는 정보를 받게 되는것을 over-fetching이라고 한다.

graphql을 쓰면 over-fetching없이 코드를 짤수 있고 개발자가 어떤 정보를 원하는 지에 대해 통제할수있다
frontend가 Database에 오직 사용자명만 요청할수있다.

under-fetching - 어떤 하나를 완성하기 위해 다른 요청들을 해야할때 발생하는데 , 예를 들어 우리가 했던 인스타그램 클론에서 앱을 시작하면서 많은 정보를 받게된다.

*요청
/피드/
/알림/
/유저/1/프로필

인스타그램 페이지의 피드도 받고,알림도 받고 사용자 플로필도 받고
앱을 처음 시작하려면 이 세가지 요청을 해야하는데, 즉 3가지 요청이 3번 오가야 앱이 시작되는것이다.
이것을 under-fetching이다 REST에서 하나를 완성하려고 많은 소스를 요청하는것이고 이것도 GraphQL이 해결할수 있는 문제다.

>>>> 그럼 한 query에서 어떻게 원하는 정보만 받을수있을까? 또한 필요한 정보만을 어떻게 알려줄수 있을까?

예를들어 피드를 원하는데 모든 사진 피드중에 댓글이랑 좋아요 수 를 원하고, 알림을 원하고, 그 알림을 확인했는지에 대한 정보를 원하며, 유저 프로필을 원하는데 사용자명과 프로필 사진을 원한다고 하자


{
    feed{ //피드정보를 원하는데 모든 사진피드중에
        comments // 댓글이랑
        likeNumber // 좋아요 갯수를 원하고
    }
    notifications{ // 알림을 원하고 
        isRead // 알림을 확는가에 대한정보를 원하고
    }
    user{ //유저프로필중에
        username// 유저이름과
        profilePic // 프로필사진을 원해
    }
}

이것을 query라고 하고

qeury{
    feed{ //피드정보를 원하는데 모든 사진피드중에
        comments // 댓글이랑
        likeNumber // 좋아요 갯수를 원하고
    }
    notifications{ // 알림을 원하고 
        isRead // 알림을 확는가에 대한정보를 원하고
    }
    user{ //유저프로필중에
        username// 유저이름과
        profilePic // 프로필사진을 원해
    }
} =>>>>> 이요청을 graphQL에 보내면



{ =>>>>> graphQL에서 javascript object로 내가 요청한것만 정확히 전달받을수있다.
    feed[
        comments:1,
        likeNumber:20
    ],
    notifications[ 
       { 
           isRead:true
       },
       {
           isRead:false
       }
    ],
    user{
        username: soonok
        profilePic:"http://블라블라"
    }
}

==>> 내가 요청한 정보들만 받을수 있고, 내가 언하는 방식으로 조정도 할수있다. API로 조정하거나 여러가질 섞어서 모양을 바꾸거나 할수 있다.

#3 Make Your First GraphQL Server

1. git 저장소 생성 해준다

리포지토리 -> 디렉토리 -> movieql 생성

2. yarn init (yarn없으면 설치후 npm install --global yarn )
    yarn init를 하면 여러가지를 묻는데 이중 description, repository,auther만 입력후 엔터
    내정보를 입력하고 디렉토리로 들어가 package.json을 확인해보면

    {
        "name": "movieql",
        "version": "1.0.0",
        "description": "Movie API With Graphql",
        "main": "index.js",
        "repository": "https://github.com/babo7179/movieql",
        "author": "babo7179<babo2722@gmail.com>",
        "license": "MIT"
    }
    정상적으로 성공한것 깃과 연결하자

    git init
    git remote add origin https://movieql저장소
    git pull origin master


3. 로컬 movieql디렉토리로 가서 graphql-yoga를 설치해주자
    yarn add graphql-yoga
=>graphql-yoga는 create-react-app명령어와 비슷하다. graphql프로젝트를 빠르게 시작할수 있게 도와준다 공식 리포지트리에는 '쉽게 설치하는데 중점을 둔 완전한 기능을 갖춘 graphQL 서버'라고 설명되어있다.

#3 서버구동해보기
yarn global add nodemon

nodemon은 우리가 파일을 수정할때마다 서버를 재 시작해준다.


