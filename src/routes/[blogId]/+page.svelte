<script>


    let blogs = [
      {
        id: 1,
        title: "How to fix req.regenerate error, when ypu log out. ",
        excerpt: "This is the first blog  the card design to display the image. Here's the revised code",
        likes: 0,
        comments: 3,
        views: 45,
        image: "/test.jpg.jpg", // Replace with your thumbnail URL
      },
    ];
  
   

   

  </script>
  
  
  
<div>
  


    <div class="outerdiv">
        <main class="blogcon">
          {#each blogs as blog}
            <div class="blog-card">
              <img class="thumbnail" src={blog.image} alt="Thumbnail for {blog.title}" />
               <!-- smal pic -->
               <a class="profilePic"  href="/profile">           
                <div style="margin-top: 4px;">
                  <!-- svelte-ignore a11y-img-redundant-alt -->
                  <img src="test.jpg.jpg" crossorigin="anonymous" alt="Profile picture" width="30px" height="30px" class="rounded-circle">
                  <small>Jeth Daspero</small>
                </div>
                </a>
              <div class="content">
                
                <h6>{blog.title}</h6>
                <p>{blog.excerpt}</p>
              </div>
              <!-- <div class="interaction-bar">
                <span class="icon">👍 {blog.likes}</span>
                <span class="icon">💬 {blog.comments}</span>
                <span class="icon">🔗 Share</span>
                <span class="icon">👀 {blog.views}</span>
              </div> -->
            </div>
          {/each}
        </main>
      </div>
</div>
 

  <style>
    .profilePic{
        color: green;
        text-decoration: none;
    }

    .blogcon{
        width: 50%;
    }
    .outerdiv{
        width: 100%;
        display: flex;
        justify-content: center;
    }
    .blog-card {
      /* border: 1px solid #ddd; */
      border-radius: 8px;
      overflow: hidden;
      margin: 16px 0;
      /* box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); */
      padding: 10px;
    }
    .thumbnail {
      width: 100%;
      height: 300px;
      object-fit: cover;
    }
    .content {
      padding: 16px;
      padding-bottom: 2px;
    }
    .content p{
      font-family: sans-serif;
    }
    .content h2{
      font-family: sans-serif;
    }

    .interaction-bar {
      display: flex;
      justify-content: space-around;
      margin-top: 4px;
      margin-bottom: 15px;
    }
    .icon {
      cursor: pointer;
      font-size: 18px;
    }

    @media only screen and (max-width:768px){
      .outerdiv{
        width: 100%;
      }
      .blogcon{
        width: 100%;
      }
      .searchinput{
        padding: 5px;
        padding-left: 5px;
      }
      .btn_search{
        margin-top: 5px;
        font-size: 15px;

      }
    }
  </style>

  