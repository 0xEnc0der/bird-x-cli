# Contributing to Bird CLI

## Setup

```bash
git clone https://github.com/0xEnc0der/bird-x-cli.git
cd bird-x-cli
pnpm install
```

## Building

```bash
rm -rf dist && npx tsc
```

## Testing

```bash
# Set up your env file first (see README.md)
bird check
bird tweet "test from bird CLI"
bird search "hello" -n 3
bird read <tweet-id>
```

## Updating Query IDs

When X rotates GraphQL query IDs:

```bash
pnpm run graphql:update
rm -rf dist && npx tsc
```

## Code Style

- TypeScript strict mode
- Use the existing mixin pattern for new features
- Run `pnpm run lint` before submitting PRs

## Pull Requests

1. Fork the repo
2. Create a feature branch
3. Make your changes
4. Test all operations (tweet, reply, read, search)
5. Submit PR with description of changes
