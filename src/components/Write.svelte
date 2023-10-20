<script>
    import { onMount } from 'svelte';
    import { navigate } from 'svelte-routing/src/history';
    import { addNewPost } from '../service/allapi';

  

    let title = '';
    let content = '';
    let authorid = '';
    let image = null;
  
    const setProfile = (e) => {
      image = e.target.files[0];
    };
  
    const handleSubmit = async (e) => {
      e.preventDefault();
  
      authorid = localStorage.getItem('userId');
  
      const formData = new FormData();
      formData.append('title', title);
      formData.append('content', content);
      formData.append('authorid', authorid);
      formData.append('image_file', image);
  
      try {
        const response = await addNewPost(formData, {
          headers: { 'Content-Type': 'multipart/form-data' }
        });
        if (response.status <= 210) {
          console.log(response);
          alert(response.data.message);
          navigate('/');
        } else {
          alert(response.data.error);
        }
      } catch (error) {
        console.error(error);
      }
    };
  </script>
  
  <div class="write">
    <div class="w-50 container">
      <img
        class="writeImg"
        src={image ? URL.createObjectURL(image) : 'https://i.postimg.cc/cJ3cSLb7/5614966-2933152.jpg'}
        alt=""
      />
      <form class="writeForm" on:submit={handleSubmit}>
        <div class="writeFormGroup">
          <label for="fileInput">
            <i class="writeIcon fas fa-plus"></i>
          </label>
          <input
            id="fileInput"
            type="file"
            style="display: none;"
            on:change={setProfile}
          />
          <input
            class="writeInput"
            placeholder="Title"
            type="text"
            bind:value={title}
            required
          />
        </div>
        <div class="writeFormGroup">
          <textarea
            class="writeInput writeText"
            placeholder="Tell your story..."
            bind:value={content}
            required
          />
        </div>
        <button class="writeSubmit" type="submit">Publish</button>
      </form>
    </div>
  </div>

  <style>
    .write {
      padding-top: 50px;
    }
  
    .writeImg {
      width: 90%;
      height: 40vh;
      border-radius: 10px;
      object-fit: cover;
    }
  
    .writeForm {
      position: relative;
    }
  
    .writeFormGroup {
      display: flex;
      align-items: center;
    }
  
    .writeIcon {
      width: 25px;
      height: 25px;
      font-size: 20px;
      border: 1px solid;
      border-radius: 50%;
      color: rgb(129, 125, 125);
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
    }
  
    .writeInput {
      font-size: 30px;
      border: none;
      padding: 20px;
      width: 70vw;
    }
  
    .writeInput::placeholder {
      color: rgb(189, 185, 185);
      font-weight: 400;
    }
  
    .writeInput:focus {
      outline-style: none;
    }
  
    .writeText {
      width: 70vw;
      height: 100vh;
      font-family: inherit;
      font-size: 20px;
    }
  
    .writeSubmit {
      position: absolute;
      top: 20px;
      right: 50px;
      color: white;
      background-color: teal;
      padding: 10px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      display: flex;
      align-items: center;
    }
  </style>
  
  