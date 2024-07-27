# How to remove a range of docker images

## All versions of an image

```bash
docker images | grep IMAGE_NAME/ANY_CUSTOM_REGEX | awk '{print $3}' | xargs docker rmi

```

## Remove a time range


1. All images in last 13 days

```bash
docker images | grep -E '([1-9]|1[0-4]) days' | awk '{print $3}' | xargs docker rmi
```

2. Specific images in last 13 days

```bash
docker images | grep IMAGE_NAME/ANY_CUSTOM_REGEX | grep -E '([1-9]|1[0-4]) days' | awk '{print $3}' | xargs docker rmi
```

3. Specific date

```bash
docker images --format "{{.Repository}}:{{.Tag}}\t{{.CreatedAt}}" | \
grep -B1 "DATE" | \
awk 'NR % 2 == 0 {print $1}' | \
xargs -r docker rmi
```

**Note:** `DATE` value should be in `%Y-%m-%d` format, like `2024-07-26`
