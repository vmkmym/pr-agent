# PR-Agent 아키텍처 분석
Pull Request(PR) 리뷰를 자동화하고 개선하기 위한 도구로, AI 모델을 활용하여 PR의 변경 사항을 분석하고 피드백을 제공합니다. 이 도구는 다양한 구성 요소로 이루어져 있으며, 각 구성 요소는 특정 역할을 담당합니다.

## 핵심 구성 요소
PRReviewer 클래스:
- PR 리뷰를 담당하는 주요 클래스입니다.
- PR의 변경 사항을 분석하고, AI 모델을 통해 피드백을 생성합니다.
- PR의 자동 승인, 라벨 설정, 증분 리뷰(incremental review) 등을 처리합니다.

AI 핸들러 (BaseAiHandler 및 LiteLLMAIHandler):
- AI 모델과의 상호작용을 담당합니다.
- PR 변경 사항을 기반으로 AI 모델에 요청을 보내고, 결과를 받아옵니다.

Git Provider:
- GitHub 또는 기타 Git 플랫폼과의 상호작용을 담당합니다.
- PR의 메타데이터, 변경된 파일, 라벨 등을 가져오거나 업데이트합니다.

설정 로더 (config_loader):
- 사용자 정의 설정을 로드하고, 이를 기반으로 PR 리뷰 동작을 제어합니다.

로깅 시스템 (log):
- PR 리뷰 과정에서 발생하는 이벤트와 오류를 기록합니다.

## 디렉토리 구조
pr-agent/
├── algo/
│   ├── ai_handlers/
│   │   ├── base_ai_handler.py  # AI 핸들러의 기본 클래스
│   │   ├── litellm_ai_handler.py  # 특정 AI 모델 핸들러
│   ├── pr_processing.py  # PR 변경 사항 처리 로직
│   ├── token_handler.py  # 토큰 처리 로직
│   ├── utils.py  # 유틸리티 함수
├── config_loader.py  # 설정 로더
├── git_providers/
│   ├── git_provider.py  # Git 플랫폼과의 상호작용
│   ├── github_provider.py  # GitHub 전용 구현
├── log.py  # 로깅 시스템
├── tools/
│   ├── pr_reviewer.py  # PR 리뷰 로직
│   ├── ticket_pr_compliance_check.py  # 티켓 준수 여부 확인
├── servers/
│   ├── help.py  # 도움말 메시지 생성

## 워크플로우
- PR 정보 수집: git_provider를 통해 PR의 변경 사항(diff), 메타데이터, 파일 목록 등을 가져옵니다.
- AI 모델 호출: LiteLLMAIHandler를 사용하여 AI 모델에 PR 변경 사항을 전달하고, 피드백을 생성합니다.
- 피드백 처리: AI 모델의 응답을 Markdown 형식으로 변환하고, PR에 코멘트로 게시합니다.
- 라벨 설정 및 자동 승인: PR의 리뷰 결과를 기반으로 라벨을 설정하거나, 조건을 만족하면 자동으로 PR을 승인합니다.
  