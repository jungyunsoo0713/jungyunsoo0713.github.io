---
title: "Terraform + ECS + GitHub Actions 빠르게 구축하기"
weight: 2
---

이번 포스트에서는 테라폼을 사용하여 Retail Store Sample App을 빠르게 구축합니다. AWS 인프라를 구축한 뒤, Retail Store Sample App을 ECS 기반으로 배포합니다. 배포 과정에서는 GitHub Actions가 적용됩니다. 이번 포스트에서 작성하는 코드는 프로덕션 환경을 위한 완성된 코드는 아닙니다. 추후 프로덕션 환경에 가까운 구성을 위해, 이 코드를 기반으로 수정과 추가 작업을 진행할 예정입니다.

---

먼저. `.gitignore` 파일을 생성합니다. `.gitignore` 파일은 GitHub에 코드를 푸시할때 제외도는 파일 확장자들을 명시합니다.

```hcl
# Local .terraform directories
**/.terraform/*

# .tfstate files
*.tfstate
*.tfstate.*

# Crash log files
crash.log
crash.*.log

# Exclude all .tfvars files, which are likely to contain sensitive data
*.tfvars
*.tfvars.json

# Ignore override files as they are usually used to override resources locally
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Ignore transient lock info files created by terraform providers
.terraform.lock.hcl

# Ignore CLI configuration files
.terraformrc
terraform.rc

# Ignore plan output files
*.tfplan

# Ignore backend config files that may contain sensitive info
backend.hcl

# OS generated files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Editor directories and files
.idea/
.vscode/
*.swp
*.swo
*~
```

