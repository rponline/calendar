<!--
  - @copyright Copyright (c) 2019 Georg Ehrke <oc.list@georgehrke.com>
  -
  - @author Georg Ehrke <oc.list@georgehrke.com>
  -
  - @license GNU AGPL version 3 or any later version
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program. If not, see <http://www.gnu.org/licenses/>.
  -
  -->

<template>
	<div class="avatar-participation-status">
		<Avatar
			:disable-tooltip="true"
			:user="avatarLink" />
		<div class="avatar-participation-status__indicator" :class="className.class" />
		<div class="avatar-participation-status__text">
			{{ className.label }}
		</div>
	</div>
</template>

<script>
import Avatar from '@nextcloud/vue/dist/Components/Avatar'

export default {
	name: 'AvatarParticipationStatus',
	components: {
		Avatar,
	},
	props: {
		avatarLink: {
			type: String,
			required: true,
		},
		participationStatus: {
			type: String,
			required: true,
		},
		commonName: {
			type: String,
			required: true,
		},
		isViewedByOrganizer: {
			type: Boolean,
			required: true,
		},
		isResource: {
			type: Boolean,
			required: true,
		},
		attendeeIsOrganizer: {
			type: Boolean,
			required: true,
		},
		organizerDisplayName: {
			type: String,
			required: true,
		},
	},
	computed: {
		className() {

			if (this.isResource && this.participationStatus === 'ACCEPTED') {
				return {
					class: ['accepted', 'icon', 'icon-checkmark-white'],
					label: t('calendar', '{name} is available.', {
					name: this.commonName,
				})
				}
			}
			if (this.isResource && this.participationStatus === 'DECLINED') {
				return {
					class: ['declined', 'icon', 'icon-close-white'],
					label: t('calendar', '{name} is not available.', {
						name: this.commonName,
					})
				}
			}

			if (this.participationStatus === 'ACCEPTED' && this.isViewedByOrganizer) {
				return {
					class: ['accepted', 'icon', 'icon-checkmark-white'],
					label: t('calendar', '{name} accepted your invitation.', {
						name: this.commonName,
					})
				}
			}
			if (this.participationStatus === 'ACCEPTED' && !this.isViewedByOrganizer) {
				return {
					class: ['accepted', 'icon', 'icon-checkmark-white'],
					label: t('calendar', '{name} accepted {organizerName}\'s invitation.', {
						name: this.commonName,
						organizerName: this.organizerDisplayName,
					})
				}
			}

			if (this.participationStatus === 'DECLINED' && this.isViewedByOrganizer) {
				return {
					class: ['declined', 'icon', 'icon-close-white'],
					label: t('calendar', '{name} declined your invitation.', {
						name: this.commonName,
					})
				}
			}
			if (this.participationStatus === 'DECLINED' && !this.isViewedByOrganizer) {
				return {
					class: ['declined', 'icon', 'icon-close-white'],
					label: t('calendar', '{name} declined {organizerName}\'s invitation.', {
						name: this.commonName,
						organizerName: this.organizerDisplayName,
					})
				}
			}

			if (this.participationStatus === 'DELEGATED') {
				return this.$t('calendar', '{name} has delegated their invitation.', {
					name: this.commonName,
				})
			}
			if (this.participationStatus === 'TENTATIVE') {
				return {
					class: ['tentative', 'icon', 'icon-checkmark-white'],
					label: t('calendar', '{name} marked their participation as tentative.', {
						name: this.commonName,
					})
				}
			}

			if (this.isViewedByOrganizer) {
				return {
					class: ['no-response', 'icon', 'icon-invitees-no-response-white'],
					label: t('calendar', '{name} did not respond to your invitation yet.', {
						name: this.commonName,
					})
				}
			} else {
				return {
					class: ['no-response', 'icon', 'icon-invitees-no-response-white'],
					label: t('calendar', '{name} did not respond to {organizerName}\'s invitation yet.', {
						name: this.commonName,
						organizerName: this.organizerDisplayName,
					})
				}
			}
		},
	},
}
</script>
<style lang="scss" scoped>
::v-deep .avatar-participation-status__indicator {
	bottom: 14px;
	left: 48px;
	opacity: .45;
	position: relative;
}
.avatar-participation-status__text {
	opacity: .45;
	left: 69px;
	bottom: 35px;
	white-space: nowrap;
	position: relative;
}
</style>
