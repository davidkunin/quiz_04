Quiz 04
=

모든 퀴즈는 해당 강의중에 언급된 내용에서만 국한된다.  
퀴즈 답변이 모호하다고 생각한다면 해당 이유를 적고 본인이 생각하는 답을 작성하여 제출한다.  

0_1. 해당 퀴즈 git을 fork하여 답안을 작성하시오.  
0_2. 운영진의 공지를 확인하고 알맞은 branch로 답지를 pull request를 보내시오.  
1. follow 기능을 어떤 gem으로 구현하였는가?  
    답 : Gem acts_as_follower

2. Comment 모델과 N:M 관계를 가지는 모델을 전부 쓰시오  
    답 : Article, artist, song, user
3. profile 유효성검사는 어떻게 하였는지 적으시오  
    답 :  validates :name, presence: true, length: { minumum:1, maximum: 30} (길이검사)
          VALID_PHONE_NUMBER = /\A010([1-9]{1}[0-9]{3})([0-9]{4})\z/
          validates :mobile, presence: true, format: {with: VALID_PHONE_NUMBER}, uniqueness: true (전화번호 형식검사)
         
4. 커스텀 helper를 사용한 기능은 무엇인지 모두 쓰시오  
    답 : 
5. 파일 업로더는 어떤 방식으로 구현하였는가  
    답 : ‘carrierwave’와 ‘mini_magick’ gem 을 통해서 만듬.

6. controller 중 rendering view 로만 이루어진것은 무엇인지 모두 쓰시오   
    답 : Follows, comments 
    
7. 강의중 게시글 본문의 에디터는 어떻게 구현하였는지 쓰시오  
    답 : Gem ’tinymce-rails’를 통해 tinymce를 사용. View 에서 <%=tinymce%> 쓰면 불러와짐. 
8. 댓글은 어떻게 구현하였는지 쓰시오   
    답 : Gem ‘act_as_commentable’ 사용
9. 컨트롤러로 넘어오는 입력값을 어떻게 가공하여 모델로 넘겨주는지 쓰시오  
    답 :  
10. 프로젝트의 root_url은 어디로 연결되어있는지 쓰시오
    답 : articles 컨트롤러 index (articles#index)
11. article_url 의 method : get 요청에는 몇개의 html 페이지가 렌더링되있는지 적고 각각이 어떤 redering page 인지 적으시오  
    답 : 
12. 본인의 보조강의 워크스페이스 github repo 주소를 적으시오  
깃헙에 없다면 새로 만들어서 링크를 제출하시오   
    답 : https://github.com/davidkunin/unilion.git
