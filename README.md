Quiz 04
=

모든 퀴즈는 해당 강의중에 언급된 내용에서만 국한된다.  
퀴즈 답변이 모호하다고 생각한다면 해당 이유를 적고 본인이 생각하는 답을 작성하여 제출한다.  

0_1. 해당 퀴즈 git을 fork하여 답안을 작성하시오.  
0_2. 운영진의 공지를 확인하고 알맞은 branch로 답지를 pull request를 보내시오.  
1. follow 기능을 어떤 gem으로 구현하였는가?  
    답 :   gem 'acts_as_follower', github: 'tcocca/acts_as_follower', branch: 'master'

2. Comment 모델과 N:M 관계를 가지는 모델을 전부 쓰시오  
    답 :      article, artist, song -  acts_as_commentable
            user, -  has_many :comments
        (comment) - belongs_to :commentable, :polymorphic => true

3. profile 유효성검사는 어떻게 하였는지 적으시오  
    답 :       before_action :write_profile, except: [:new, :create]

4. 커스텀 helper를 사용한 기능은 무엇인지 모두 쓰시오  
    답 :   
1) jukebox에서 user가 admin이거나 editor인 경우 수정 가능하도록 하는 함수
 module JukeboxHelper
    def user_editable?
        if current_user.profile.role=='admin'
            true
        elsif current_user.profile.role =='editor'
            true
        else
            false
        end
    end        
end
2) profile에서 gravatar를 이용하여 이메일과 연동된 사진 불러오기 기능
module ProfilesHelper
  def gravatar_url(email, size)
    gravatar = Digest::MD5::hexdigest(email).downcase
    url = "http://gravatar.com/avatar/#{gravatar}.png?s=#{size}"
  end
end

5. 파일 업로더는 어떤 방식으로 구현하였는가  
    답 :   
gem 'carrierwave'
#파일 업로더 만들어줌
gem 'mini_magick'
#올라온 이미지 편집해줌
두 가지 gem을 이용해서 view에서 image를 받을 수 있도록 구현

        <%= f.file_field :image, accept: 'image/jpeg, image/jpg, image/gif, image/png'%>
 <script>
    $('#artist_image').on('change',function(){
        var fileName = $(this).val().replace(/^.*[\\\/]/, '');
        $(this).next('.custom-file-label').html(fileName);
    })
 </script>
6. controller 중 rendering view 로만 이루어진것은 무엇인지 모두 쓰시오   
    답 :   comments, follows

7. 강의중 게시글 본문의 에디터는 어떻게 구현하였는지 쓰시오  
    답 :   
gem 'tinymce-rails' 설치해주고
articles의 views에
<%= f.text_area :content, class:'tinymce', rows: 15%>
        <%= tinymce %> 를 통해 에디터를 구현할 수 있다.


8. 댓글은 어떻게 구현하였는지 쓰시오   
    답 : 
1) gem 'acts_as_commentable' 설치해준다.
2) comment controller를 만들어준다.
class CommentsController < ApplicationController
    before_action :authenticate_user!
    before_action :set_comment, only: [:destroy]
    def create
        @comment = Comment.new(comment_params)
        @comment.user = current_user
        @comment.save
        redirect_to @comment.commentable
    end
    
    def destroy
        @comment.destroy
        redirect_to @comment.commentable
    end
    
    private
    def set_comment
       @comment = Comment.find params[:id] 
    end
    
    def comment_params
       params.require(:comment).permit(:content, :commentable_id, :commentable_type) 
    end

end
3) comment.rb 모델을 만들어준다. 
class Comment < ActiveRecord::Base

  include ActsAsCommentable::Comment
  
  belongs_to :commentable, :polymorphic => true

  default_scope -> { order('created_at ASC') }

  belongs_to :user
  validates :content, presence: :true, length:{minimum:2, maximum:200}

end

4) m:n 모델 관계를 설정해준다.
article, artist, song -  acts_as_commentable
            user, -  has_many :comments
        (comment) - belongs_to :commentable, :polymorphic => true

5) comment view를 만들어주고, 필요한 view(article, artist, song)에 render 해준다.

//comment view - form
<hr />
<%= bootstrap_form_for Comment.new do |f| %>
    <%= f.hidden_field :commentable_id, value: commentable.id %>
    <%= f.hidden_field :commentable_type, value: commentable.class %>

    <%= f.text_field :content %>
    <%= f.submit %>
<% end %>

//comment view - index
<% commentable.comments.each do |comment|%>
    <div class="card comment-card">
      <h5 class="card-header">
        <%= comment.user.profile.name %>
        <%= link_to fa_icon('trash'), comment_path(comment), method: :delete, data: {confirm: 'comment를 삭제합니다.'}%>
        </h5>
      <div class="card-body">
        <p class="card-text">
            <%= comment.content%>
        
        </p>
      </div>
    </div>
<% end %>

// artist, article, song의 show.html.erb
<%= render 'comments/form', commentable: @artist %>
<hr />
<%= render 'comments/index', commentable: @artist %>



9. 컨트롤러로 넘어오는 입력값을 어떻게 가공하여 모델로 넘겨주는지 쓰시오  
    답 :  

private
  def set_article
    @article=Article.find(params[:id])
  end
  
  def article_params
    params.require(:article).permit(:title, :content)
  end
 end
10. 프로젝트의 root_url은 어디로 연결되어있는지 쓰시오
    답 : 
  root to: 'articles#index'

11. article_url 의 method : get 요청에는 몇개의 html 페이지가 렌더링되있는지 적고 각각이 어떤 redering page 인지 적으시오  
    답 : views의 articles의  _like.html.erb, _userUI.html.erb, 
views의 comments의 _form.html.erb, index.html.erb
-> 4개

<h1><%=@article.title%></h1>
<%= render 'userUI'%>
<%= render 'like'%>
<hr />
<p> <%= @article.content.html_safe %></p>
<%= render 'comments/form', commentable: @article %>
<hr />
<%= render 'comments/index', commentable: @article %>

12. 본인의 보조강의 워크스페이스 github repo 주소를 적으시오  
깃헙에 없다면 새로 만들어서 링크를 제출하시오   
    답 : https://github.com/eunjin97/quiz04rapp.git - clone하는 주소
https://github.com/eunjin97/quiz04rapp - 그냥 주소