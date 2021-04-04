# Hugo Blog

## Chekcout
```shell
$ git clone --recurse-submodules -j8 git@github.com:iamkyu/hugo-blog.git
$ git submodule add -b master git@github.com:iamkyu/iamkyu.github.io.git public
```

## 로컬에서 실행
```shell
$ hugo server -w
```

## 테마 업데이트
```shell
$ git submodule update --remote
```

## 정적컨텐츠 업로드
```shell
$ ./deploy.sh "commit msg"
```
- 현재는 Github Action 을 통해 정적컨텐츠 자동 업로드 되므로 직접 커밋 불필요. 
