<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BipinBook</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #f0f2f5;
    }
    .navbar {
      background-color: #1877f2;
      padding: 10px 20px;
      color: white;
      font-size: 22px;
      font-weight: bold;
    }
    .container {
      max-width: 600px;
      margin: 20px auto;
    }
    .post-box, .post, .comment, .reply-box {
      background: white;
      padding: 10px 15px;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      margin-bottom: 15px;
    }
    .post-box textarea, .comment-input textarea, .reply-input textarea {
      width: 100%;
      border: 1px solid #ccc;
      border-radius: 5px;
      outline: none;
      resize: none;
      font-size: 14px;
      padding: 5px;
      margin-top: 5px;
    }
    .post-box button, .comment-input button, .reply-input button {
      background-color: #1877f2;
      color: white;
      border: none;
      padding: 6px 12px;
      border-radius: 8px;
      cursor: pointer;
      margin-top: 5px;
    }
    .post .author, .comment .author {
      font-weight: bold;
      margin-bottom: 5px;
    }
    .actions, .comment-actions, .reply-actions {
      display: flex;
      gap: 10px;
      font-size: 14px;
      margin-top: 5px;
    }
    .actions button, .comment-actions button, .reply-actions button {
      background: none;
      border: none;
      color: #555;
      cursor: pointer;
    }
    .actions button:hover, .comment-actions button:hover, .reply-actions button:hover {
      color: #1877f2;
    }
    .counters, .comment-counters {
      font-size: 13px;
      color: #888;
      margin-top: 5px;
    }
    .comments-section, .replies-section {
      margin-top: 10px;
      padding-left: 15px;
    }
    .comment, .reply-box {
      margin-top: 5px;
    }
    .reply-input {
      margin-top: 5px;
      margin-left: 20px;
    }
  </style>
</head>
<body>
  <div class="navbar">BipinBook</div>

  <div class="container">
    <!-- Post Box -->
    <div class="post-box">
      <textarea id="postInput" rows="3" placeholder="What's on your mind?"></textarea>
      <button onclick="addPost()">Post</button>
    </div>

    <!-- Feed -->
    <div id="feed">
      <div class="post">
        <div class="author">Bipin Hamal</div>
        <div>Hello friends! Welcome to my new page 😊</div>
        <div class="actions">
          <button onclick="likePost(this)">👍 Like</button>
          <button onclick="showCommentBox(this)">💬 Comment</button>
          <button>↗ Share</button>
        </div>
        <div class="counters">
          <span class="likes" data-count="0">0 Likes</span> • 
          <span class="comments" data-count="0">0 Comments</span> • 
          <span class="views">1 View</span>
        </div>
        <div class="comments-section"></div>
      </div>
    </div>
  </div>

<script>
  function addPost() {
    let input = document.getElementById("postInput");
    if (input.value.trim() === "") return;

    let feed = document.getElementById("feed");
    let newPost = document.createElement("div");
    newPost.className = "post";
    newPost.innerHTML = `
      <div class="author">You</div>
      <div>${input.value}</div>
      <div class="actions">
        <button onclick="likePost(this)">👍 Like</button>
        <button onclick="showCommentBox(this)">💬 Comment</button>
        <button>↗ Share</button>
      </div>
      <div class="counters">
        <span class="likes" data-count="0">0 Likes</span> • 
        <span class="comments" data-count="0">0 Comments</span> • 
        <span class="views">1 View</span>
      </div>
      <div class="comments-section"></div>
    `;
    feed.prepend(newPost);
    input.value = "";
  }

  function likePost(btn) {
    let likesSpan = btn.closest(".post").querySelector(".likes");
    let count = parseInt(likesSpan.dataset.count);
    if (btn.style.color === "blue") {
      btn.style.color = "#555";
      count--;
    } else {
      btn.style.color = "blue";
      count++;
    }
    likesSpan.dataset.count = count;
    likesSpan.textContent = `${count} Likes`;
  }

  function showCommentBox(btn) {
    let post = btn.closest(".post");
    if (post.querySelector(".comment-input")) return;

    let commentBox = document.createElement("div");
    commentBox.className = "comment-input";
    commentBox.innerHTML = `
      <textarea rows="2" placeholder="Write a comment..."></textarea>
      <button onclick="addComment(this)">Post Comment</button>
    `;
    post.appendChild(commentBox);
  }

  function addComment(btn) {
    let commentBox = btn.parentElement;
    let post = btn.closest(".post");
    let textarea = commentBox.querySelector("textarea");
    let commentText = textarea.value.trim();
    if (!commentText) return;

    let commentsSection = post.querySelector(".comments-section");
    let newComment = document.createElement("div");
    newComment.className = "comment";
    newComment.innerHTML = `
      <div class="author">You:</div>
      <div>${commentText}</div>
      <div class="comment-actions">
        <button onclick="likeComment(this)">👍 Like</button>
        <button onclick="showReplyBox(this)">↩ Reply</button>
      </div>
      <div class="comment-counters">
        <span class="likes" data-count="0">0 Likes</span> • 
        <span class="replies" data-count="0">0 Replies</span>
      </div>
      <div class="replies-section"></div>
    `;
    commentsSection.appendChild(newComment);

    // Update comment count
    let commentsSpan = post.querySelector(".comments");
    let count = parseInt(commentsSpan.dataset.count);
    count++;
    commentsSpan.dataset.count = count;
    commentsSpan.textContent = `${count} Comments`;

    commentBox.remove();
  }

  function likeComment(btn) {
    let likesSpan = btn.closest(".comment").querySelector(".comment-counters .likes");
    let count = parseInt(likesSpan.dataset.count);
    if (btn.style.color === "blue") {
      btn.style.color = "#555";
      count--;
    } else {
      btn.style.color = "blue";
      count++;
    }
    likesSpan.dataset.count = count;
    likesSpan.textContent = `${count} Likes`;
  }

  function showReplyBox(btn) {
    let comment = btn.closest(".comment");
    if (comment.querySelector(".reply-input")) return;

    let replyBox = document.createElement("div");
    replyBox.className = "reply-input";
    replyBox.innerHTML = `
      <textarea rows="2" placeholder="Write a reply..."></textarea>
      <button onclick="addReply(this)">Post Reply</button>
    `;
    comment.appendChild(replyBox);
  }

  function addReply(btn) {
    let replyBox = btn.parentElement;
    let comment = btn.closest(".comment");
    let textarea = replyBox.querySelector("textarea");
    let replyText = textarea.value.trim();
    if (!replyText) return;

    let repliesSection = comment.querySelector(".replies-section");
    let newReply = document.createElement("div");
    newReply.className = "reply-box";
    newReply.innerHTML = `
      <div class="author">You:</div>
      <div>${replyText}</div>
      <div class="reply-actions">
        <button onclick="likeReply(this)">👍 Like</button>
      </div>
      <div class="comment-counters">
        <span class="likes" data-count="0">0 Likes</span>
      </div>
    `;
    repliesSection.appendChild(newReply);

    // Update replies count
    let repliesSpan = comment.querySelector(".comment-counters .replies");
    let count = parseInt(repliesSpan.dataset.count);
    count++;
    repliesSpan.dataset.count = count;
    repliesSpan.textContent = `${count} Replies`;

    replyBox.remove();
  }

  function likeReply(btn) {
    let likesSpan = btn.closest(".reply-box").querySelector(".comment-counters .likes");
    let count = parseInt(likesSpan.dataset.count);
    if (btn.style.color === "blue") {
      btn.style.color = "#555";
      count--;
    } else {
      btn.style.color = "blue";
      count++;
    }
    likesSpan.dataset.count = count;
    likesSpan.textContent = `${count} Likes`;
  }
</script>
</body>
</html>
