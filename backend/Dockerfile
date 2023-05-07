FROM node:13-alpine

WORKDIR /usr/backend

RUN apk --no-cache add curl

# Install dependencies
COPY package.json package-lock.json ./
RUN npm install
# Copy application code
COPY . .

CMD ["sh", "-c", "npm run start"]