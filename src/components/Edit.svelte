<script>
    import { onMount } from "svelte";
    import { Link, navigate } from "svelte-routing";
    import { editPostContent, singlePost } from "../service/allapi";
    import { useParams } from "svelte-routing";

    const params = useParams();

    let id = $params.id;
    let post = { title: "", content: "", image: "", date: "" };

    onMount(async () => {
        const response = await singlePost(id);
        if (response.data) {
            post = response.data;
        }
    });
    console.log(id);

    const editpost = (e) => {
        const { name, value } = e.target;
        post = { ...post, [name]: value };
    };

    const handleSubmit = async () => {
        try {
            const response = await editPostContent(post, id);
            if (response.status <= 210) {
                console.log(response);
                alert(response.data.message);
                navigate(`/view/${post._id}`);
            } else {
                alert(response.data.error);
            }
        } catch (error) {
            console.error(error);
        }
    };
</script>

<div class="view-post-container">
    <div class="post mt-5">
        <img class="postImg" src={`BASE_URL}/uploads/${post.image}`} alt="" />

        <div class="postInfo">
            <span class="postTitle">
                <input
                    class="link text-center"
                    name="title"
                    bind:value={post.title}
                    on:input={editpost}
                />
            </span>
            <hr />
            <span class="postDate me-5 ms-auto">{post.date}</span>
        </div>
        <textarea
            class="postDesc edit-input"
            name="content"
            bind:value={post.content}
            on:input={editpost}
        />
        <div class="button-container ms-auto mt-3">
            <button
                on:click={handleSubmit}
                class="edit-button edit-edit-button text-success"
            >
                Save changes
            </button>
        </div>
    </div>
</div>

<style>
    .edit-edit-button {
        border: none;
        font-size: 20px;
    }
    .view-post-container {
        display: flex;
        justify-content: space-evenly;
        flex-wrap: wrap;
        margin-top: 50px;
    }
</style>
