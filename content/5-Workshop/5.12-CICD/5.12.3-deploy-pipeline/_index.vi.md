---
title : "Triá»ƒn khai liÃªn tá»¥c lÃªn ECS"
date : 2026-07-10
weight : 3
chapter : false
pre : " <b> 5.12.3. </b> "
---

Sau khi Docker Image má»›i Ä‘Æ°á»£c Ä‘Ã³ng gÃ³i vÃ  Ä‘áº©y thÃ nh cÃ´ng lÃªn Amazon ECR á»Ÿ bÃ i 5.12.2, bÆ°á»›c cuá»‘i cÃ¹ng cá»§a quy trÃ¬nh CI/CD lÃ  tá»± Ä‘á»™ng cáº­p nháº­t há»‡ thá»‘ng mÃ¡y chá»§ Amazon ECS Ä‘á»ƒ cháº¡y phiÃªn báº£n mÃ£ nguá»“n má»›i nháº¥t.

Äá»ƒ Ä‘áº£m báº£o há»‡ thá»‘ng khÃ´ng bá»‹ giÃ¡n Ä‘oáº¡n (Zero-downtime) trong quÃ¡ trÃ¬nh cáº­p nháº­t, nhÃ³m dá»± Ã¡n thiáº¿t káº¿ luá»“ng triá»ƒn khai dá»±a trÃªn cÆ¡ cháº¿ **Rolling Update** máº·c Ä‘á»‹nh cá»§a Amazon ECS Fargate.

#### BÆ°á»›c 1: Cáº­p nháº­t luá»“ng Workflow trÃªn GitHub Actions
Má»Ÿ láº¡i tá»‡p `.github/workflows/deploy.yml` Ä‘Ã£ táº¡o á»Ÿ bÃ i trÆ°á»›c trÃªn VS Code vÃ  bá»• sung thÃªm cÃ¡c bÆ°á»›c (Steps) táº£i cáº¥u hÃ¬nh Task Definition hiá»‡n táº¡i, cáº­p nháº­t Image ID má»›i, vÃ  ra lá»‡nh triá»ƒn khai trá»±c tiáº¿p lÃªn ECS Cluster.

Ná»™i dung tá»‡p `deploy.yml` hoÃ n chá»‰nh sau khi tÃ­ch há»£p luá»“ng CD:

```yaml
name: Deploy Backend to ECR and ECS

on:
  push:
    branches:
      - main

permissions:
  id-token: write 
  contents: read 

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Source Code
        uses: actions/checkout@v4

      - name: Configure AWS Credentials via OIDC
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::236320489525:role/GitHubActionsDeployRole
          aws-region: ap-southeast-1

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build, tag, and push Docker image to ECR
        id: build-image
        env:
          REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          REPOSITORY: cloudforge-backend
          IMAGE_TAG: ${{ github.sha }}
        run: |
          cd backend
          docker build -t $REGISTRY/$REPOSITORY:$IMAGE_TAG .
          docker push $REGISTRY/$REPOSITORY:$IMAGE_TAG
          echo "image=$REGISTRY/$REPOSITORY:$IMAGE_TAG" >> $GITHUB_OUTPUT

      - name: Download current ECS task definition
        run: |
          aws ecs describe-task-definition \
            --task-definition cloudforge-backend-task \
            --query taskDefinition > task-definition.json

      - name: Fill in the new image ID in the Amazon ECS task definition
        id: task-def
        uses: aws-actions/amazon-ecs-render-task-definition@v1
        with:
          task-definition: task-definition.json
          container-name: backend-container
          image: ${{ steps.build-image.outputs.image }}

      - name: Deploy Amazon ECS task definition
        uses: aws-actions/amazon-ecs-deploy-task-definition@v2
        with:
          task-definition: ${{ steps.task-def.outputs.task-definition }}
          service: cloudforge-backend-service
          cluster: cloudforge-compute-cluster
          wait-for-service-stability: true
```

**Giáº£i thÃ­ch luá»“ng triá»ƒn khai bá»• sung:**
- **Download task definition:** Sá»­ dá»¥ng AWS CLI (Ä‘Ã£ Ä‘Æ°á»£c xÃ¡c thá»±c qua OIDC ngáº¯n háº¡n) Ä‘á»ƒ táº£i xuá»‘ng cáº¥u hÃ¬nh Task Definition Ä‘ang hoáº¡t Ä‘á»™ng (Active) dÆ°á»›i dáº¡ng tá»‡p `task-definition.json`, giÃºp Pipeline luÃ´n Ä‘á»“ng bá»™ vá»›i cáº¥u hÃ¬nh thá»±c táº¿ trÃªn háº¡ táº§ng.
- **render-task-definition:** TÃ¬m kiáº¿m container cÃ³ tÃªn `cloudforge-backend-container` bÃªn trong tá»‡p Ä‘á»‹nh nghÄ©a JSON vá»«a táº£i vÃ  thay tháº¿ trÆ°á»ng `image` báº±ng Ä‘Æ°á»ng dáº«n URI cá»§a Docker Image chá»©a mÃ£ SHA commit vá»«a Ä‘Æ°á»£c Ä‘áº©y lÃªn ECR.
- **deploy-task-definition:** ÄÄƒng kÃ½ má»™t phiÃªn báº£n má»›i (New Revision) cá»§a Task Definition lÃªn AWS vÃ  cáº­p nháº­t ECS Service Ä‘á»ƒ kÃ­ch hoáº¡t tiáº¿n trÃ¬nh Rolling Update. Tham sá»‘ `wait-for-service-stability: true` buá»™c GitHub Actions giá»¯ káº¿t ná»‘i chá» cho Ä‘áº¿n khi cÃ¡c container má»›i vÆ°á»£t qua Health Check á»•n Ä‘á»‹nh thÃ¬ má»›i Ä‘Ã³ng phiÃªn cháº¡y thÃ nh cÃ´ng.

#### BÆ°á»›c 2: Quan sÃ¡t tiáº¿n trÃ¬nh Rolling Update
LÆ°u láº¡i tá»‡p cáº¥u hÃ¬nh trÃªn mÃ¡y cá»¥c bá»™, thá»±c hiá»‡n commit vÃ  `git push` lÃªn nhÃ¡nh `main`.

1. **TrÃªn GitHub Actions:** Theo dÃµi tiáº¿n trÃ¬nh CI/CD. Giai Ä‘oáº¡n *Deploy Amazon ECS task definition* sáº½ duy trÃ¬ tráº¡ng thÃ¡i cháº¡y khoáº£ng 3 Ä‘áº¿n 5 phÃºt Ä‘á»ƒ Ä‘á»£i ECS container khá»Ÿi táº¡o xong.
2. **TrÃªn AWS ECS Console:** Chuyá»ƒn sang giao diá»‡n quáº£n lÃ½ cá»¥m mÃ¡y chá»§, truy cáº­p vÃ o dá»‹ch vá»¥ `cloudforge-backend-service` vÃ  chá»n tháº» **Deployments**.

Táº¡i Ä‘Ã¢y, cÆ¡ cháº¿ Rolling Update sáº½ diá»…n ra má»™t cÃ¡ch tá»± Ä‘á»™ng vÃ  trá»±c quan:
- ECS tá»± Ä‘á»™ng tÃ­nh toÃ¡n vÃ  khá»Ÿi táº¡o (Provision) cÃ¡c tÃ¡c vá»¥ Task má»›i song song vá»›i cÃ¡c Task cÅ© dá»±a trÃªn Image má»›i Ä‘Æ°á»£c chá»‰ Ä‘á»‹nh.
- CÃ¡c Task má»›i tá»± Ä‘á»™ng Ä‘Äƒng kÃ½ vÃ o Target Group cá»§a Application Load Balancer (ALB) vÃ  thá»±c hiá»‡n quÃ¡ trÃ¬nh kiá»ƒm tra sá»©c khá»e Ä‘áº§u vÃ o.
- Khi cÃ¡c Task má»›i Ä‘áº¡t tráº¡ng thÃ¡i RUNNING vÃ  HEALTHY, ECS sáº½ tá»« tá»« Ä‘iá»u hÆ°á»›ng toÃ n bá»™ lÆ°u lÆ°á»£ng ngÆ°á»i dÃ¹ng sang phiÃªn báº£n má»›i, Ä‘á»“ng thá»i rÃºt vÃ  há»§y (Deregister) cÃ¡c Task cÅ©, Ä‘áº£m báº£o há»‡ thá»‘ng duy trÃ¬ hoáº¡t Ä‘á»™ng liÃªn tá»¥c 100% khÃ´ng xuáº¥t hiá»‡n lá»—i giÃ¡n Ä‘oáº¡n.

![ECS Rolling Update](/images/5-Workshop/5.12-CICD/5.12.3-deploy-pipeline/5.12.3-ecs-deploy.png)

***

**Tá»•ng káº¿t ChÆ°Æ¡ng 5.12:** NhÃ³m dá»± Ã¡n Ä‘Ã£ thiáº¿t láº­p thÃ nh cÃ´ng má»™t quy trÃ¬nh CI/CD Pipeline Ä‘áº¡t tiÃªu chuáº©n báº£o máº­t Enterprise. Ká»ƒ tá»« nay, báº¥t ká»³ thay Ä‘á»•i nÃ o trÃªn mÃ£ nguá»“n Backend cÅ©ng sáº½ tá»± Ä‘á»™ng Ä‘i qua luá»“ng khÃ©p kÃ­n: **XÃ¡c thá»±c OIDC báº£o máº­t -> Tá»± Ä‘á»™ng Ä‘Ã³ng gÃ³i vÃ  Ä‘áº©y Image -> Triá»ƒn khai Rolling Update khÃ´ng giÃ¡n Ä‘oáº¡n dá»‹ch vá»¥**. VÃ²ng Ä‘á»i phÃ¡t triá»ƒn pháº§n má»m (SDLC) Ä‘Ã£ Ä‘Æ°á»£c tá»‘i Æ°u hÃ³a tá»± Ä‘á»™ng hÃ³a hoÃ n toÃ n.

**BÆ°á»›c tiáº¿p theo:** Há»‡ thá»‘ng Ä‘Ã£ tá»± váº­n hÃ nh hoÃ n háº£o, nhÆ°ng lÃ m sao Ä‘á»ƒ quáº£n trá»‹ viÃªn theo dÃµi Ä‘Æ°á»£c sá»©c khá»e há»‡ thá»‘ng vÃ  truy váº¿t lá»—i náº¿u cÃ³ sá»± cá»‘ xáº£y ra? á»ž chÆ°Æ¡ng cuá»‘i cÃ¹ng (**ChÆ°Æ¡ng 5.13: Observability**), chÃºng ta sáº½ khÃ¡m phÃ¡ cÃ¡ch giÃ¡m sÃ¡t toÃ n diá»‡n á»©ng dá»¥ng vá»›i AWS CloudWatch vÃ  AWS X-Ray.


