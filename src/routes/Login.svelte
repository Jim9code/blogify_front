<script>
  const serverUrl = import.meta.env.VITE_SERVER_URL
  const google_login_url = `${serverUrl}/auth/google`

</script>

<div class="outdiv">
    <div class="inner_div">
        <center> <h6>Sign in or Sign up with google 👍</h6></center>
        <br>
        <br><br><br>
        <center>
            <a href={google_login_url}>
                <span style="color: blue;">G</span><span>o</span><span style="color:orange;">o</span><span style="color:blue;">g</span><span style="color:green;">l</span>e
            </a>
        </center>

        

    </div>
</div>

<style>
    .outdiv{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 60vh;
    }

    a{
        border: 1px solid black;
        padding: 20px;
        padding-left: 60px;
        padding-right: 60px;
        text-decoration: none;
        color: orangered;
        font-weight: bolder;
        border-radius: 8px;
    }

    .inner_div{
        background: transparent;
        height: 300px;
        padding: 10px;
        width: 50%;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    } 

    @media only screen and (max-width:768px){
        .inner_div{
            height: 300px;
        padding: 10px;
        width: auto;
        }
    }
</style>