<template>
  <f7-page>
    <f7-navbar title="Messsages" back-link="Back"></f7-navbar>

    <f7-messagebar
      :placeholder="placeholder"
      ref="messagebar"
      :attachmentsVisible="attachmentsVisible"
      :sheetVisible="sheetVisible"
    >
      <f7-link
        icon-if-ios="f7:camera_fill"
        icon-if-md="f7:camera_alt"
        slot="inner-start"
        @click="sheetToggle"
      ></f7-link>
      <f7-link
        icon-if-ios="f7:arrow_up_fill"
        icon-if-md="material:send"
        slot="inner-end"
        @click="sendMessage"
      ></f7-link>
      <f7-messagebar-attachments>
        <f7-messagebar-attachment
          v-for="(image, index) in attachments"
          :key="index"
          :image="image"
          @attachment:delete="deleteAttachment(image)"
        ></f7-messagebar-attachment>
      </f7-messagebar-attachments>
      <f7-messagebar-sheet>
        <f7-messagebar-sheet-image
          v-for="(image, index) in images"
          :key="index"
          :image="image"
          :checked="attachments.indexOf(image) >= 0"
          @change="handleAttachment"
        ></f7-messagebar-sheet-image>
      </f7-messagebar-sheet>
    </f7-messagebar>

    <f7-messages ref="messages">
      <f7-messages-title><b>Sunday, Feb 9,</b> 12:58</f7-messages-title>
      <f7-message
        v-for="(message, index) in messagesData"
        :key="index"
        :type="message.type"
        :text="message.text"
        :image="message.image"
        :name="message.name"
        :avatar="message.avatar"
        :first="isFirstMessage(message, index)"
        :last="isLastMessage(message, index)"
        :tail="isTailMessage(message, index)"
      ></f7-message>
    </f7-messages>
  </f7-page>
</template>
<script>
  import { f7Navbar, f7Page, f7Messages, f7MessagesTitle, f7Message, f7Messagebar, f7Link, f7MessagebarAttachments, f7MessagebarAttachment, f7MessagebarSheet, f7MessagebarSheetImage } from 'framework7-vue';

  export default {
    components: {
      f7Navbar,
      f7Page,
      f7Messages,
      f7MessagesTitle,
      f7Message,
      f7Messagebar,
      f7MessagebarAttachments,
      f7MessagebarAttachment,
      f7MessagebarSheet,
      f7MessagebarSheetImage,
      f7Link,
    },
    data() {
      return {
        attachments: [],
        sheetVisible: false,
        messagesData: [
          {
            type: 'sent',
            text: 'Hi, Kate',
          },
          {
            type: 'sent',
            text: 'How are you?',
          },
          {
            name: 'Kate',
            type: 'received',
            text: 'Hi, I am good!',
            avatar: 'http://lorempixel.com/100/100/people/9',
          },
          {
            name: 'Blue Ninja',
            type: 'received',
            text: 'Hi there, I am also fine, thanks! And how are you?',
            avatar: 'http://lorempixel.com/100/100/people/7',
          },
          {
            type: 'sent',
            text: 'Hey, Blue Ninja! Glad to see you ;)',
          },
          {
            type: 'sent',
            text: 'Hey, look, cutest kitten ever!',
          },
          {
            type: 'sent',
            image: 'http://lorempixel.com/200/260/cats/4/',

          },
          {
            name: 'Kate',
            type: 'received',
            text: 'Nice!',
            avatar: 'http://lorempixel.com/100/100/people/9',
          },
          {
            name: 'Kate',
            type: 'received',
            text: 'Like it very much!',
            avatar: 'http://lorempixel.com/100/100/people/9',
          },
          {
            name: 'Blue Ninja',
            type: 'received',
            text: 'Awesome!',
            avatar: 'http://lorempixel.com/100/100/people/7',
          },
        ],
        images: [
          'http://lorempixel.com/300/300/cats/1/',
          'http://lorempixel.com/200/300/cats/2/',
          'http://lorempixel.com/400/300/cats/3/',
          'http://lorempixel.com/300/150/cats/4/',
          'http://lorempixel.com/150/300/cats/5/',
          'http://lorempixel.com/300/300/cats/6/',
          'http://lorempixel.com/300/300/cats/7/',
          'http://lorempixel.com/200/300/cats/8/',
          'http://lorempixel.com/400/300/cats/9/',
          'http://lorempixel.com/300/150/cats/10/',
        ],
        people: [
          {
            name: 'Kate Johnson',
            avatar: 'http://lorempixel.com/100/100/people/9',
          },
          {
            name: 'Blue Ninja',
            avatar: 'http://lorempixel.com/100/100/people/7',
          },
        ],
        answers: [
          'Yes!',
          'No',
          'Hm...',
          'I am not sure',
          'And what about you?',
          'May be ;)',
          'Lorem ipsum dolor sit amet, consectetur',
          'What?',
          'Are you sure?',
          'Of course',
          'Need to think about it',
          'Amazing!!!',
        ],
        responseInProgress: false,
      };
    },
    // Resize page when attachments or sheet become visible
    watch: {
      attachmentsVisible() {
        const self = this;
        self.$nextTick(() => {
          self.messagebar.resizePage();
        });
      },
      sheetVisible() {
        const self = this;
        self.$nextTick(() => {
          self.messagebar.resizePage();
        });
      },
    },
    computed: {
      attachmentsVisible() {
        const self = this;
        return self.attachments.length > 0;
      },
      placeholder() {
        const self = this;
        return self.attachments.length > 0 ? 'Add comment or Send' : 'Message';
      },
    },
    methods: {
      isFirstMessage(message, index) {
        const self = this;
        const previousMessage = self.messagesData[index - 1];
        if (message.isTitle) return false;
        if (!previousMessage || previousMessage.type !== message.type || previousMessage.name !== message.name) return true;
        return false;
      },
      isLastMessage(message, index) {
        const self = this;
        const nextMessage = self.messagesData[index + 1];
        if (message.isTitle) return false;
        if (!nextMessage || nextMessage.type !== message.type || nextMessage.name !== message.name) return true;
        return false;
      },
      isTailMessage(message, index) {
        const self = this;
        const nextMessage = self.messagesData[index + 1];
        if (message.isTitle) return false;
        if (!nextMessage || nextMessage.type !== message.type || nextMessage.name !== message.name) return true;
        return false;
      },
      sheetToggle() {
        const self = this;
        self.sheetVisible = !self.sheetVisible;
      },
      deleteAttachment(image) {
        const self = this;
        const index = self.attachments.indexOf(image);
        self.attachments.splice(index, 1)[0]; // eslint-disable-line
      },
      handleAttachment(e) {
        const self = this;
        const index = self.$$(e.target).parents('label.checkbox').index();
        const image = self.images[index];
        if (e.target.checked) {
          // Add to attachments
          self.attachments.unshift(image);
        } else {
          // Remove from attachments
          self.attachments.splice(self.attachments.indexOf(image), 1);
        }
      },
      sendMessage() {
        const self = this;
        const text = self.messagebar.getValue().replace(/\n/g, '<br>').trim();
        const messagesToSend = [];
        self.attachments.forEach((attachment) => {
          messagesToSend.push({
            image: attachment,
          });
        });
        if (text.trim().length) {
          messagesToSend.push({
            text,
          });
        }
        // Reset attachments
        self.attachments = [];
        // Hide sheet
        self.sheetVisible = false;
        // Clear area
        self.messagebar.clear();
        // Focus area
        if (text.length) self.messagebar.focus();
        // Send message
        self.messagesData.push(...messagesToSend);

        // Mock response
        if (self.responseInProgress) return;
        self.responseInProgress = true;
        setTimeout(() => {
          const answer = self.answers[Math.floor(Math.random() * self.answers.length)];
          const person = self.people[Math.floor(Math.random() * self.people.length)];
          self.messages.showTyping({
            header: `${person.name} is typing`,
            avatar: person.avatar,
          });
          setTimeout(() => {
            self.messagesData.push({
              text: answer,
              type: 'received',
              name: person.name,
              avatar: person.avatar,
            });
            self.messages.hideTyping();
            self.responseInProgress = false;
          }, 4000);
        }, 1000);
      },
      onF7Ready() {
        const self = this;
        self.messagebar = self.$refs.messagebar.f7Messagebar;
        self.messages = self.$refs.messages.f7Messages;
      },
    },
  };
</script>
