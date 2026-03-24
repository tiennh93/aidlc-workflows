# Hướng dẫn dành cho Nhà phát triển

## Chạy CodeBuild Cục bộ

Bạn có thể chạy các bản dựng AWS CodeBuild cục bộ bằng cách sử dụng [CodeBuild local agent](https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html). Điều này rất hữu ích để kiểm tra các thay đổi của buildspec trước khi đẩy lên remote.

### Điều kiện Tiên quyết

- Docker đã được cài đặt và đang chạy
- Tập lệnh `codebuild_build.sh`:

### Cách sử dụng Cơ bản

1. Thiết lập
- Tải xuống tập lệnh CodeBuild cục bộ và cấp quyền thực thi.
- Gửi GitHub Personal Access Token (PAT) dùng cho biến môi trường `GH_TOKEN` vào tệp `./.env`

```bash
if [ ! -f codebuild_build.sh ]; then
  curl -O https://raw.githubusercontent.com/aws/aws-codebuild-docker-images/master/local_builds/codebuild_build.sh && chmod +x codebuild_build.sh;
fi;
echo "GH_TOKEN=${GH_TOKEN:-ghp_notset}" > "./.env";
```

2. Lặp lại (Iterate)

- _Tùy chọn chỉnh sửa giá trị `buildspec-override` trong GitHub workflow tại `.github/workflows/codebuild.yml`_
- Cập nhật `./buildspec.yml` dựa trên nội dung workflow vào một tệp cục bộ
- Chạy bản dựng AWS CodeBuild cục bộ với các image dựa trên kiến trúc máy

```bash
cat .github/workflows/codebuild.yml \
    | uvx yq -r '.jobs.build.steps[] | select(.id == "codebuild") | .with["buildspec-override"]' \
    > buildspec.yml
./codebuild_build.sh \
  -i "public.ecr.aws/codebuild/amazonlinux-$([ "$(arch)" = "arm64" -o "$(arch)" = "aarch64" ] && echo "aarch64" || echo "x86_64")-standard:$([ "$(arch)" = "arm64" -o "$(arch)" = "aarch64" ] && echo "3.0" || echo "5.0")" \
  -a "./.codebuild/artifacts/" \
  -l "public.ecr.aws/codebuild/local-builds:$([ "$(arch)" = "arm64" -o "$(arch)" = "aarch64" ] && echo "aarch64" || echo "latest")" \
  -c \
  -e "./.env"
```

### Tất cả các Tùy chọn Tập lệnh

| Cờ (Flag)    | Bắt buộc | Mô tả                                                                                                                                                                                               |
|--------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-i IMAGE`   | Có       | Image container bản dựng (ví dụ: `aws/codebuild/standard:5.0`)                                                                                                                                      |
| `-a DIR`     | Có       | Thư mục đầu ra của artifact                                                                                                                                                                         |
| `-b FILE`    | Không    | Tệp ghi đè buildspec. Mặc định là `buildspec.yml` trong thư mục nguồn                                                                                                                               |
| `-s DIR`     | Không    | Thư mục nguồn. Cờ `-s` đầu tiên là nguồn chính; các cờ `-s` bổ sung sử dụng định dạng `<sourceIdentifier>:<sourceLocation>` cho các nguồn phụ. Mặc định là thư mục làm việc hiện tại                |
| `-l IMAGE`   | Không    | Ghi đè image local agent mặc định                                                                                                                                                                   |
| `-r DIR`     | Không    | Thư mục đầu ra của báo cáo                                                                                                                                                                          |
| `-c`         | Không    | Sử dụng cấu hình và thông tin xác thực AWS từ máy chủ cục bộ của bạn (các biến môi trường `~/.aws` và `AWS_*`)                                                                                      |
| `-p PROFILE` | Không    | AWS CLI profile để sử dụng (yêu cầu `-c`)                                                                                                                                                           |
| `-e FILE`    | Không    | Tệp chứa các biến môi trường (định dạng `VAR=VAL`, mỗi biến trên một dòng)                                                                                                                          |
| `-m`         | Không    | Mount trực tiếp thư mục nguồn vào build container                                                                                                                                                   |
| `-d`         | Không    | Chạy build container trong chế độ Docker privileged                                                                                                                                                 |


## Chạy GitHub Actions cục bộ

_LƯU Ý: Điều này sử dụng công cụ [`act`](https://github.com/nektos/act) và giả định có quyền truy cập vào một dự án AWS CodeBuild hợp lệ `codebuild-project` ở "us-east-1"_

```shell
act --platform ubuntu-latest=-self-hosted \
    --job build \
    --workflows .github/workflows/codebuild.yml \
    --env-file .env \
    --var CODEBUILD_PROJECT_NAME=codebuild-project \
    --var AWS_REGION=us-east-1 \
    --var ROLE_DURATION_SECONDS=7200 \
    --artifact-server-path=$PWD/.codebuild/artifacts \
    --cache-server-path=$PWD/.codebuild/artifacts \
    --env ACT_CODEBUILD_DIR=$PWD/.codebuild/downloads \
    --bind
```
