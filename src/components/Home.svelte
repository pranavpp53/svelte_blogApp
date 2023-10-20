<script>
  import { onMount } from "svelte";
  import { allPosts } from "../service/allapi";
  import BASE_URL from "../service/baseurl";
  import { Link } from "svelte-routing";

  let allBlogs = [];
  let currentPage = 1;
  const postsPerPage = 5;
  let lastThreeBlogs = [];

  async function allPostsCall() {
    const response = await allPosts();
    allBlogs = response.data;
  }

  function formatDateToDaysAgo(dateString) {
    const date = new Date(dateString);
    const currentDate = new Date();
    const timeDifference = currentDate - date;
    const minutesDifference = Math.floor(timeDifference / (1000 * 60));

    if (minutesDifference < 60) {
      return `${minutesDifference} minutes ago`;
    } else if (minutesDifference < 1440) {
      // Less than 24 hours
      const hoursDifference = Math.floor(minutesDifference / 60);
      return `${hoursDifference} hours ago`;
    } else if (minutesDifference < 2880) {
      // Less than 48 hours
      return "Yesterday";
    } else {
      const daysDifference = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
      return `${daysDifference} days ago`;
    }
  }

  onMount(async () => {
    await allPostsCall();
    lastThreeBlogs = allBlogs.slice(-3);
  });

  let indexOfLastPost = currentPage * postsPerPage;
  let indexOfFirstPost = indexOfLastPost - postsPerPage;
  let currentPosts = allBlogs.slice(indexOfFirstPost, indexOfLastPost);

  function paginate(pageNumber) {
    currentPage = pageNumber;
  }
</script>

<div>
  <div class="body-m">
    <div class="slider">
      <input type="radio" name="toggle" id="btn-1" checked />
      <input type="radio" name="toggle" id="btn-2" />
      <input type="radio" name="toggle" id="btn-3" />

      <div class="slider-controls">
        <label for="btn-1" />
        <label for="btn-2" />
        <label for="btn-3" />
      </div>

      <ul class="slides w-100">
        {#each lastThreeBlogs as blog (blog._id)}
          <li class="slide">
            <div class="slide-content">
              <h2 class="slide-title">{blog.title}</h2>
              <p class="slide-description">{blog.content}</p>
              <p class="slide-link">
                <Link
                  style=" text-decoration: none; color: black;"
                  to={`/view/${blog._id}`}>Learn more</Link
                >
              </p>
            </div>
            <p class="slide-image">
              <img
                src={`${BASE_URL}/uploads/${blog.image}`}
                alt="stuff"
                width="320"
                height="240"
              />
            </p>
          </li>
        {/each}
      </ul>
    </div>
  </div>

  <!-- Display current page posts -->
  {#each currentPosts as i (i._id)}
    <Link style=" text-decoration: none;" to={`/post/${i._id}`}>
      <div class="home-post-container">
        <div class="post mt-5">
          <img class="postImg" src={`${BASE_URL}/uploads/${i.image}`} alt="" />
          <div class="postInfo">
            <span class="postTitle">
              <Link style="color: blue; text-decoration: none;" class="link"
                >{i.title}</Link
              >
            </span>
            <hr />
            <div class="d-flex">
              <span class="postDate me-5">Author : {i.author.username}</span>
              <span class="postDate ms-5"
                >Posted : {formatDateToDaysAgo(i.date)}</span
              >
            </div>
          </div>
          <p class="postDesc">{i.content}</p>
        </div>
      </div>
    </Link>
  {/each}

  <!-- Pagination -->
  <div class="w-100 container">
    <div class="pagination">
      {#each Array(Math.ceil(allBlogs.length / postsPerPage)) as _, index}
        <button
          key={index}
          on:click={() => paginate(index + 1)}
          class="page-button {currentPage === index + 1 ? 'active' : ''}"
        >
          {index + 1}
        </button>
      {/each}
    </div>
  </div>
</div>

<style>
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

  .postDesc {
    font-family: "Varela Round", Arial, Helvetica, sans-serif;
    font-weight: 400;
    font-size: 14px;
    line-height: 24px;
    color: #444444;
    margin-top: 15px;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 4;
    -webkit-box-orient: vertical;
  }

  .home-post-container {
    display: flex;
    justify-content: space-evenly;
    flex-wrap: wrap;
    border-bottom: 1px solid rgb(229, 226, 226);
    width: 100%;
  }

  .pagination {
    display: flex;
    justify-content: center;
  }

  .page-button {
    width: 2rem;
    height: 2.5rem;
    margin-right: 0.25rem;
    margin-top: 10px;
    margin-bottom: 10px;
    display: inline-flex;
    flex-shrink: 0;
    justify-content: center;
    align-items: center;
    border-radius: 10px;
    font-size: 20px;
    border: 1px solid rgb(144, 144, 180);
    color: rgb(58, 58, 104);
  }

  img {
    background-size: cover;
  }

  .body-m {
    font-size: 16px;
    line-height: 1.5;
    font-family: Roboto, sans-serif;
  }

  .slider {
    position: relative;
    width: 960px;
    height: 300px;
    margin: 50px auto;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12),
      0 3px 1px -2px rgba(0, 0, 0, 0.2);
    overflow: hidden;
  }

  .slider-controls {
    position: absolute;
    bottom: 0px;
    left: 50%;
    width: 200px;
    text-align: center;
    transform: translatex(-50%);
    z-index: 1000;

    list-style: none;
    text-align: center;
  }

  .slider input[type="radio"] {
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0;
    width: 0;
    height: 0;
  }

  .slider-controls label {
    display: inline-block;
    border: none;
    height: 20px;
    width: 20px;
    border-radius: 50%;
    cursor: pointer;
    background-color: #212121;
    transition: background-color 0.2s linear;
  }

  #btn-1:checked ~ .slider-controls label[for="btn-1"] {
    background-color: #ff4081;
  }

  #btn-2:checked ~ .slider-controls label[for="btn-2"] {
    background-color: #ff4081;
  }

  #btn-3:checked ~ .slider-controls label[for="btn-3"] {
    background-color: #ff4081;
  }

  /* SLIDES */

  .slides {
    list-style: none;
    padding: 0;
    margin: 0;
    height: 100%;
    width: 100%;
  }

  .slide {
    position: absolute;
    top: 0;
    left: 0;

    display: flex;
    justify-content: space-between;
    padding: 20px;
    width: 100%;
    height: 100%;

    opacity: 0;
    transform: translatex(-100%);
    transition: transform 250ms linear;
  }

  .slide-content {
    width: 400px;
  }

  .slide-title {
    margin-bottom: 20px;
    font-size: 36px;
  }

  .slide-description {
    font-family: "Varela Round", Arial, Helvetica, sans-serif;
    font-weight: 400;
    font-size: 14px;
    line-height: 24px;
    color: #444444;
    margin-top: 20px;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 4;
    -webkit-box-orient: vertical;
  }

  .slide-link {
    display: inline-block;
    padding: 10px 20px;
    color: #fff;
    border-radius: 3px;
    text-decoration: none;
    background-color: #ff4081;
  }

  .slide-image img {
    max-width: 100%;
  }

  /* Slide animations */
  #btn-1:checked ~ .slides .slide:nth-child(1) {
    transform: translatex(0);
    opacity: 1;
  }

  #btn-2:checked ~ .slides .slide:nth-child(2) {
    transform: translatex(0);
    opacity: 1;
  }

  #btn-3:checked ~ .slides .slide:nth-child(3) {
    transform: translatex(0);
    opacity: 1;
  }

  #btn-1:not(:checked) ~ .slides .slide:nth-child(1) {
    animation-name: swap-out;
    animation-duration: 300ms;
    animation-timing-function: linear;
  }

  #btn-2:not(:checked) ~ .slides .slide:nth-child(2) {
    animation-name: swap-out;
    animation-duration: 300ms;
    animation-timing-function: linear;
  }

  #btn-3:not(:checked) ~ .slides .slide:nth-child(3) {
    animation-name: swap-out;
    animation-duration: 300ms;
    animation-timing-function: linear;
  }

  @keyframes swap-out {
    0% {
      transform: translatex(0);
      opacity: 1;
    }

    50% {
      transform: translatex(50%);
      opacity: 0;
    }

    100% {
      transform: translatex(100%);
    }
  }
</style>
