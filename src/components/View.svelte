<script>
  import { onMount } from 'svelte';
  import { Link } from 'svelte-routing';
  import { singlePost } from '../service/allapi';
  import BASE_URL from '../service/baseurl';

  import { useParams } from "svelte-routing";

const params = useParams(); 


  let post = [];
  let id = $params.id;

  function formatDateToDaysAgo(dateString) {
    const date = new Date(dateString);
    const currentDate = new Date();
    const timeDifference = currentDate - date;
    const minutesDifference = Math.floor(timeDifference / (1000 * 60));

    if (minutesDifference < 60) {
      return `${minutesDifference} minutes ago`;
    } else if (minutesDifference < 1440) { // Less than 24 hours
      const hoursDifference = Math.floor(minutesDifference / 60);
      return `${hoursDifference} hours ago`;
    } else if (minutesDifference < 2880) { // Less than 48 hours
      return "Yesterday";
    } else {
      const daysDifference = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
      return `${daysDifference} days ago`;
    }
  }

  onMount(async () => {
    try {
      const { data } = await singlePost(id);
      post = data;
    } catch (error) {
      console.error(error);
    }
  });
</script>

<div class="view-post-container">
  <div class="post mt-5">
    <img class="postImg" src={`${BASE_URL}/uploads/${post.image}`} alt="" />

    <div class="postInfo">
      <span class="postTitle">
        <Link to="#" class="link">{post.title}</Link>
      </span>
      <hr />
      <span class="postDate me-5 ms-auto">{formatDateToDaysAgo(post.date)}</span>
    </div>
    <p class="view-postDesc">{post.content}</p>
  </div>
</div>

<style>
  .view-post-container {
    display: flex;
    justify-content: space-evenly;
    flex-wrap: wrap;
    margin-top: 50px;
  }

  .view-postDesc {
    font-family: "Varela Round", Arial, Helvetica, sans-serif;
    font-weight: 400;
    font-size: 14px;
    line-height: 24px;
    color: #444444;
    margin-top: 15px;
  }

  .post {
    width: 30rem;
    margin: 0px 25px 40px 25px;
    display: flex;
    flex-direction: column;
  }

  .postImg {
    width: 100%;
    height: 280px;
    object-fit: cover;
    border-radius: 7px;
  }

  .postInfo {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .postTitle {
    font-family: "Josefin Sans", Arial, Helvetica, sans-serif;
    font-size: 24px;
    font-weight: 900;
    margin-top: 10px;
    cursor: pointer;
  }

  .postDate {
    font-family: "Lora", serif;
    font-style: italic;
    font-size: 21px;
    font-weight: 400;
    color: #5c5a5a;
  }
</style>
