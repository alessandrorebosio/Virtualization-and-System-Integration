# 1. Base stage - Installazione dipendenze
FROM node:20-alpine AS base
WORKDIR /app
RUN apk add --no-cache libc6-compat
COPY package.json package-lock.json* ./

# 2. Development stage
FROM base AS dev
RUN npm ci
COPY . .
COPY .env/.env.development .env/.env.development 
EXPOSE 3000
CMD ["npm", "run", "dev"]

# 3. Builder stage
FROM base AS builder
COPY . .
COPY .env/.env.production .env/.env.production
RUN npm ci
ENV NEXT_TELEMETRY_DISABLED=1
RUN npm run build

# 4. Production Runner stage
FROM node:20-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public

COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

COPY --from=builder /app/.env/.env.production .env/.env.production

USER nextjs
EXPOSE 3000
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

CMD ["node", "server.js"]