<template>
	<div>
		<h2>{{ post.title }}</h2>
		<p>{{ post.content }}</p>
		<p class="text-muted">{{ post.createdAt }}</p>
		<hr class="my-4" />
		<div class="row g-2">
			<div class="col-auto">
				<button class="btn btn-outline-dark">이전글</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-dark">다음글</button>
			</div>
			<div class="col-auto me-atuo"></div>
			<div class="col-auto">
				<button class="btn btn-outline-dark" @click="goListPage">목록</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-primary" @click="goEditPage">
					수정
				</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-danger" @click="remove">삭제</button>
			</div>
		</div>
		<!-- <p>{{ $route.params }}</p> -->
		<!-- <p>{{ $route.query }}</p> -->
		<!-- <p>{{ $route.hash }}</p> -->
	</div>
</template>

<script setup>
import { useRoute, useRouter } from 'vue-router';
import { getPostById, deletePost } from '@/api/posts.js';
import { ref } from 'vue';

const props = defineProps({
	id: Number,
});

// const route = useRoute();
const router = useRouter();
// const id = route.params.id;
const post = ref({});

const fetchPost = async () => {
	try {
		const { data } = await getPostById(props.id);
		console.log('data :', data);
		setPost(data);
	} catch (e) {
		console.error(e);
	}
};

const setPost = ({ title, content, createAt }) => {
	post.value.title = title;
	post.value.content = content;
	post.value.createAt = createAt;
};

fetchPost();

const remove = async () => {
	try {
		if (!confirm('삭제하시겠습니까?')) return;
		await deletePost(props.id);
		router.push({ name: 'PostList' });
	} catch (e) {
		console.error(e);
	}
};

const goListPage = () => {
	router.push({
		name: 'PostList',
	});
};
const goEditPage = () => {
	router.push({
		name: 'PostEdit',
		params: {
			id: props.id,
		},
	});
};
</script>

<style lang="scss" scoped></style>
