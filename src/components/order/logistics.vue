<template>
	<div class="logist-wrap">
		<div v-title :data-title="title">{{title}}</div>
		<logis-header :text="title"></logis-header>
		<div class="status-wrap">
			<div class="list-hide-top"></div>
			<div class="list-hide-bottom"></div>
			<div class="list-item" :class="{active:index == 0}" v-for="(item,index) in $store.state.logis_data.Traces" :key="index">
				<div class="status-icon-wrap">
					<span class="icon"></span>
				</div>
				<div class="hd clearfix">
					<span class="fl time">{{item.AcceptTime}}</span>
					<span class="fr name"></span>
					<span class="icon"><i></i></span>
				</div>
				<p class="con">{{item.AcceptStation}}</p>
			</div>
		</div>
	</div>
</template>
<script>
	import logisHeader from '@/components/page/children/header.vue';
	import qs from 'qs';
	export default {
		name: 'logistics',
		data() {
			return {
				title: '物流查询',
			}
		},
		components: {
			logisHeader
		},
		mounted() {
			this.axios.post(this.$httpConfig.lookGoodsExpress, qs.stringify({
				id: this.$route.params.id,
				order_status:this.$route.params.order_status,
			})).then((res) => {
				if(res.data.status == 10001) {
					this.$router.push('/LogIn');
				} else if(res.data.status == 1){
					let r=JSON.parse(res.data.data);
							r.Traces.reverse();
					this.$store.commit('logis',r);
				}
			}).catch((err) => {
				console.log(err);
			});
		}
	}
</script>
<style lang="less" scoped>
	.status-wrap {
		margin: 0 .2rem 0 .35rem;
		border-left: 1px solid #bbb;
		position: relative;
		padding-bottom: 1rem;
		.list-item {
			padding: .2rem;
			margin-top: .4rem;
			margin-left: .35rem;
			border: 1px solid #d2d2d2;
			border-radius: 5px;
			width: 6.16rem;
			position: relative;
			background: #fff;
			.status-icon-wrap {
				position: absolute;
				left: -.74rem;
				top: 0;
				width: .74rem;
				height: 100%;
				.icon {
					position: absolute;
					left: .253rem;
					top: .2rem;
					width: .21rem;
					height: .21rem;
					background: #fff;
					border: .05rem solid #b5b5b5;
					box-sizing: border-box;
					border-radius: 50%;
				}
			}
			.hd {
				span {
					color: #999;
					font-size: .25rem;
				}
				position:relative;
				span.icon {
					position: absolute;
					left: -.3rem;
					top: .1rem;
					width: .2rem;
					height: .2rem;
					border: 1px solid #ccc;
					transform: rotate(45deg);
					background: #fff;
					box-sizing: border-box;
					i {
						position: absolute;
						top: -4px;
						right: -4px;
						width: .2rem;
						height: .2rem;
						border: 1px solid #fff;
						background: #fff;
					}
				}
			}
			.con {
				font-size: .3rem;
				color: #333;
				padding: .2rem 0;
				line-height: .4rem;
			}
		}
		.list-item.active {
			.con {
				color: #e02828;
			}
			.status-icon-wrap {
				.icon {
					background: #e02828;
					border-color: #e02828;
					box-shadow: 0 0 .1rem .05rem rgba(224, 40, 40, .5);
				}
			}
		}
		.list-hide-top {
			width: .6rem;
			height: .22rem;
			background: #f1f1f1;
			position: absolute;
			top: 0%;
			left: -.35rem;
		}
		.list-hide-bottom {
			position: absolute;
			bottom: 0;
			left: -.35rem;
			width: .6rem;
			height: 2.2rem;
			background: #f1f1f1;
		}
	}
</style>