<template>
	<v-app id="inspire">
		<v-main>
			<v-container
				fluid
			>
				<v-row
					align="center"
					justify="center"
				>
					<v-col
						cols="12"
						sm="8"
						md="4"
					>
						<v-card class="elevation-12">
							<v-toolbar
								dark
								flat
							>
								<v-toolbar-title>Login</v-toolbar-title>
							</v-toolbar>
							<v-card-text>
								<v-form>
									<v-text-field
										label="Username"
										name="login"
										prepend-icon="mdi-account"
										type="text"
									></v-text-field>

									<v-text-field
										id="password"
										label="Password"
										name="password"
										prepend-icon="mdi-lock"
										type="password"
									></v-text-field>
								</v-form>
							</v-card-text>
							<v-card-actions>
								<v-spacer></v-spacer>
								<v-btn 
									dark
									@click="login"
								>Login</v-btn>
							</v-card-actions>
						</v-card>
					</v-col>
				</v-row>
			</v-container>
		</v-main>
	</v-app>
</template>

<script>
	export default {
		props: {
			loggedIn: Boolean
		},

		methods: {
			login () {
				this.loggedIn = true;
				this.$emit('userLoggedIn', this.loggedIn);
			}
		}
	};
</script>