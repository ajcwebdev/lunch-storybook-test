# Redwood Tutorial App

This repo represents the final state of the app created during the [Redwood Tutorial](https://redwoodjs.com/tutorial).
It is meant to be a starting point for those working on the [Tutorial Part 2](https://redwoodjs.com/tutorial2).

This repo contains much more styling than the one we built together in the tutorial, but is functionally identical.

## Setup

The [tutorial itself](https://redwoodjs.com/tutorial2/prerequisites) contains instructions for getting this repo up and running, but here is a summary of the commands:

### Clone example blog repo

```bash
git clone https://github.com/redwoodjs/redwood-tutorial
```

### Change directory and install dependencies

```bash
cd redwood-tutorial && yarn
```

### Generate the Prisma client and apply migrations

```bash
yarn rw db up
```

### Seed database with test data and start development server

```bash
yarn rw db seed && yarn rw dev
```

### Start Storybook

```bash
yarn rw storybook
```

## Our First Story

### BlogPost.js

```javascript
// web/src/components/BlogPost/BlogPost.js

import { Link, routes } from '@redwoodjs/router'

const BlogPost = ({ post }) => {
  return (
    <article>
      <header>
        <h2 className="text-xl text-blue-700 font-semibold">
          <Link to={routes.blogPost({ id: post.id })}>{post.title}</Link>
        </h2>
      </header>
      <div className="mt-2 text-gray-900 font-light">{post.body}</div>
    </article>
  )
}

export default BlogPost
```

```javascript
<article>
  <header>
    <h2>
      <Link to={
        routes.blogPost({ id: post.id })
      }>
        {post.title}
      </Link>
    </h2>
  </header>
  
  <div>{post.body}</div>
</article>
```

### BlogPost.stories.js

```javascript
// web/src/components/BlogPost/BlogPost.stories.js

import BlogPost from './BlogPost'

export const generated = () => {
  return (
    <BlogPost
      post={{
        id: 1,
        title: 'First Post',
        body:
          'Neutra tacos hot chicken prism raw denim, put a bird on it enamel pin post-ironic vape cred DIY. Street art next level umami squid. Hammock hexagon glossier 8-bit banjo. Neutra la croix mixtape echo park four loko semiotics kitsch forage chambray. Semiotics salvia selfies jianbing hella shaman. Letterpress helvetica vaporware cronut, shaman butcher YOLO poke fixie hoodie gentrify woke heirloom.',
        createdAt: '2020-01-01T12:34:56Z',
      }}
    />
  )
}

export default { title: 'Components/BlogPost' }
```

### Create `truncate` function

```javascript
// web/src/components/BlogPost/BlogPost.js

const truncate = (text, length) => {
  return text.substring(0, length) + '...'
}
```

### Truncate `body` of the `post` to `100`

```javascript
// web/src/components/BlogPost/BlogPost.js

const BlogPost = ({ post, summary = false }) => {
  return (
    <article className="mt-10">
      <header>
        <h2 className="text-xl text-blue-700 font-semibold">
          <Link to={routes.blogPost({ id: post.id })}>{post.title}</Link>
        </h2>
      </header>
      <div className="mt-2 text-gray-900 font-light">
        {summary ? truncate(post.body, 100) : post.body}
      </div>
    </article>
  )
}
```

```javascript
<article>
  <header>
    <h2>
      <Link to={
        routes.blogPost({ id: post.id })
      }>
        {post.title}
      </Link>
    </h2>
  </header>
  
  <div>
    {summary ? truncate(post.body, 100) : post.body}
  </div>
</article>
```

### Edit `BlogPost.stories.js` to include `summary` and `full`

```javascript
// web/src/components/BlogPost/BlogPost.stories.js

import BlogPost from './BlogPost'

const POST = {
  id: 1,
  title: 'First Post',
  body: `Neutra tacos hot chicken prism raw denim, put a bird on it
         enamel pin post-ironic vape cred DIY. Street art next level
         umami squid. Hammock hexagon glossier 8-bit banjo. Neutra
         la croix mixtape echo park four loko semiotics kitsch forage
         chambray. Semiotics salvia selfies jianbing hella shaman.
         Letterpress helvetica vaporware cronut, shaman butcher YOLO
         poke fixie hoodie gentrify woke heirloom.`
,
}
export const full = () => {
  return <BlogPost post={POST} />
}
export const summary = () => {
  return <BlogPost post={POST} summary={true} />
}

export default { title: 'Components/BlogPost' }
```

### Set `summary` to `true` in `BlogPostsCell`

```javascript
// web/src/components/BlogPostsCell/BlogPostsCell.js

export const Success = ({ posts }) => {
  return (
    <div className="-mt-10">
      {posts.map((post) => (
        <div key={post.id} className="mt-10">
          <BlogPost post={post} summary={true} />
        </div>
      ))}
    </div>
  )
}
```

## Build a Component the Redwood🅪 Way

### Create a component to display a comment

```bash
yarn rw g component Comment
```

### Add `comment` object to `Comment` component

```javascript
// web/src/components/Comment/Comment.js

const Comment = ({ comment }) => {
  return (
    <div>
      <h2>
        {comment.name}
      </h2>
      
      <time datetime={comment.createdAt}>
        {comment.createdAt}
      </time>
      
      <p>
        {comment.body}
      </p>
    </div>
  )
}

export default Comment
```

### Update story to include comment object

```javascript
// web/src/components/Comment/Comment.stories.js

export const generated = () => {
  return (
    <Comment
      comment={{
        name: 'Rob Cameron',
        body: 'This is the first comment!',
        createdAt: '2020-01-01T12:34:56Z'
      }}
    />
  )
}
```

### Format date

```javascript
// web/src/components/Comment/Comment.js

const formattedDate = (datetime) => {
  const parsedDate = new Date(datetime)
  const month = parsedDate.toLocaleString('default', { month: 'long' })
  return `${parsedDate.getDate()} ${month} ${parsedDate.getFullYear()}`
}
```

### Include `formattedDate` in `Comment`

```javascript
// web/src/components/Comment/Comment.js

const Comment = ({ comment }) => {
  return (
    <div>
      <h2>
        {comment.name}
      </h2>
      
      <time
        className="text-xs text-gray-500"
        dateTime={comment.createdAt}
      >
        {formattedDate(comment.createdAt)}
      </time>
        
      <p>
        {comment.body}
      </p>
    </div>
  )
}
```

### Add styling to `Comment`

```javascript
// web/src/components/Comment/Comment.js

const Comment = ({ comment }) => {
  return (
    <div className="bg-gray-200 p-8 rounded-lg">
      <header className="flex justify-between">
        <h2 className="font-semibold text-gray-700">
          {comment.name}
        </h2>
        
        <time
          className="text-xs text-gray-500"
          dateTime={comment.createdAt}
        >
          {formattedDate(comment.createdAt)}
        </time>  
      </header>
      
      <p className="text-sm mt-2">
        {comment.body}
      </p>
    </div>
  )
}
```

### Add `margin` in the story

```javascript
// web/src/components/Comment/Comment.stories.js

export const generated = () => {
  return (
    <div className="m-4">
      <Comment
        comment={{
          name: 'Rob Cameron',
          body: 'This is the first comment!',
          createdAt: '2020-01-01T12:34:56Z',
        }}
      />
    </div>
  )
}
```

## Multiple Comments

### Generate `CommentsCell`

```
yarn rw g cell Comments
```

### Import `Comment` into `CommentsCell`

```javascript
// web/src/components/CommentsCell/CommentsCell.js

import Comment from 'src/components/Comment'
```

### Include comment information in `comments` query

```javascript
// web/src/components/CommentsCell/CommentsCell.js

export const QUERY = gql`
  query CommentsQuery {
    comments {
      id
      name
      body
      createdAt
    }
  }
`
```

### Return `Comment` component in `Success`

```javascript
// web/src/components/CommentsCell/CommentsCell.js

export const Success = ({ comments }) => {
  return comments.map((comment) => (
    <Comment
      key={comment.id}
      comment={comment}
    />
  ))
}
```

### Update `CommentsCell.mock.js` to return an array

```javascript
// web/src/components/CommentsCell/CommentsCell.mock.js

export const standard = () => ({
  comments: [
    {
      id: 1,
      name: 'Rob Cameron',
      body: 'First comment',
      createdAt: '2020-01-02T12:34:56Z',
    },
    {
      id: 2,
      name: 'David Price',
      body: 'Second comment',
      createdAt: '2020-02-03T23:00:00Z',
    },
  ],
})
```

### Add margin between comments

```javascript
// web/src/components/CommentsCell/CommentsCell.js

export const Success = ({ comments }) => {
  return (
    <div className="-mt-8">
      {comments.map((comment) => (
        <div
          key={comment.id}
          className="mt-8"
        >
          <Comment comment={comment} />
        </div>
      ))}
    </div>
  )
}
```

### Add margin to story

```javascript
export const success = () => {
  return Success ? (
    <div className="m-8 mt-16">
      <Success {...standard()} />
    </div>
  ) : null
}
```

### Import `CommentsCell` to `BlogPost`

```javascript
// web/src/components/BlogPost/BlogPost.js

import CommentsCell from 'src/components/CommentsCell'
```

### Return `CommentsCell` below the `body`

```javascript
// web/src/components/BlogPost/BlogPost.js

const BlogPost = ({ post, summary = false }) => {
  return (
    <article className="mt-10">
      <header>
        <h2 className="text-xl text-blue-700 font-semibold">
          <Link to={
            routes.blogPost({ id: post.id })
          }>
            {post.title}
          </Link>
        </h2>
      </header>
      
      <div className="mt-2 text-gray-900 font-light">
        {summary ? truncate(post.body, 100) : post.body}
      </div>
      
      {!summary && <CommentsCell />}
    </article>
  )
}
```

### Add `decorators` key so all stories have margin

```javascript
// web/src/components/BlogPost/BlogPost.stories.js

export default {
  title: 'Components/BlogPost',
  decorators: [
    (Story) => <div className="m-8"><Story /></div>
  ]
}
```

### Add gap between end of blog post and start of comments

```javascript
// web/src/components/BlogPost/BlogPost.js

const BlogPost = ({ post, summary = false }) => {
  return (
    <article className="mt-10">
      <header>
        <h2 className="text-xl text-blue-700 font-semibold">
          <Link to={
            routes.blogPost({ id: post.id })
          }>
            {post.title}
          </Link>
        </h2>
      </header>
      
      <div className="mt-2 text-gray-900 font-light">
        {summary ? truncate(post.body, 100) : post.body}
      </div>
      
      {!summary && (
        <div className="mt-24">
          <CommentsCell />
        </div>
      )}
    </article>
  )
}
```
