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
