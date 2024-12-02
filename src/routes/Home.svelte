<script>


    let blogs = [
      {
        id: 1,
        title: "First Blog Post",
        excerpt: "This is the first blog  the card design to display the image. Here's the revised code",
        likes: 0,
        comments: 3,
        views: 45,
        image: "/test.jpg.jpg", // Replace with your thumbnail URL
      },
      {
        id: 2,
        title: "Second Blog Post",
        excerpt: "Another interesting post.",
        likes: 0,
        comments: 5,
        views: 120,
        image: "https://via.placeholder.com/500x500", // Replace with your thumbnail URL
      },
    ];
  
    function likePost(id) {
      const blog = blogs.find((b) => b.id === id);
      if (blog) blog.likes++;
    }

   

  </script>
  
  
  
<div>
  
    <div class="searchdiv">
      <form >
        <center>
            <input class="searchinput" type="search" placeholder="🔍 Search" > 
            <button class="btn_search" type="submit">Search</button>
        </center>
      </form>
    </div>

    <div class="outerdiv">
        <main class="blogcon">
          {#each blogs as blog}
            <div class="blog-card">
              <img class="thumbnail" src={blog.image} alt="Thumbnail for {blog.title}" />
              <div class="content">
                <h5>{blog.title}</h5>
                <!-- <p>{blog.excerpt}</p> -->
              </div>
              <div class="interaction-bar">
                <span class="icon">👍 {blog.likes}</span>
                <!-- <span class="icon">💬 {blog.comments}</span> -->
                <span class="icon">🔗 Share</span>
                <span class="icon">👀 {blog.views}</span>
              </div>
            </div>
          {/each}
        </main>
      </div>
</div>
 

  <style>
    .btn_search{
        background: rgba(50, 205, 50, 0.1);
        border-radius: 8px;
        padding: 7px;
        font-weight: bolder;
        border: none;
        font-size: 20px;
    }

  form{
  display: flex;
  justify-content: space-between;
}

  .searchinput{
  padding: 7.5px;
  width: 70%;
  border-radius: 8px;
  }.searchinput:focus{
    border: 1px solid rgba(50, 205, 50, 0.696);
  }
    .searchdiv{
        display: flex;
        justify-content: center;
        padding: 30px;
        position: sticky;
        top: 0px;
        z-index: 2000;
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

  