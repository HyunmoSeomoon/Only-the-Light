# 코드 검토 기록

검토일: 2026-11-05

## 수행한 검증

- 원본 Assets 아래 C# 112개를 Roslyn 파서로 검사: 구문 오류 0개.
- 게시 대상 게임 스크립트 13개에서 비밀키·비밀번호·이메일·사용자 절대경로 패턴 검색: 발견 0건. 이 검색이 모든 형태의 비밀정보 부재를 보장하지는 않습니다.
- 게시한 소스 및 메타데이터를 원본과 SHA-256으로 비교: 일치.
- 정확한 Unity 버전인 6000.0.39f1이 로컬에 없어 Unity 컴파일·빌드·플레이 검증은 수행하지 않았습니다.

## 정적 검토에서 확인한 사항

아래 항목은 수정·동작 검증 전의 기록입니다. 원본 스크립트는 변경하지 않았습니다.

| 항목 | 확인 내용 |
| --- | --- |
| 컴포넌트 파일명 | BookHeadLogic.cs, DemonDollLogic.cs, ZombieLogic.cs의 MonoBehaviour 클래스명이 각각 EnemyLightZoneDetector, EnemyDetector, ZombieLightZoneDetector로 파일명과 다릅니다. 원본 씬에서 해당 스크립트 GUID가 참조되므로 Unity의 실제 로드 여부를 확인해야 합니다. |
| 씬 전환 | GameSceneManager.LoadMainGameScene은 MainGameScene을 로드하지만 원본 씬·빌드 목록은 JJH3입니다. 해당 메서드 호출 시 전환 실패 가능성이 있습니다. |
| 디버그 입력 | DisappearOnWeaponHit의 T 키 입력으로 전체 몬스터 사망 상태가 활성화됩니다. 배포 동작인지 확인이 필요합니다. |
| 재시작 상태 | static InteractionManager.letterCount가 ResetGameState에서 초기화되지 않습니다. EnemyManager는 씬 전환 후 몬스터 참조 갱신도 확인해야 합니다. |
| 근접 판정 | PlayerState는 IsChasingPlayer가 참일 때만 사망 거리를 확인하지만, 그 함수는 attackDistance 이내를 제외합니다. 공격 범위로 바로 진입하는 경로의 판정을 확인해야 합니다. |
| 필수 참조 | PlayerFlashlight 등은 인스펙터 및 싱글톤 참조를 전제로 하므로 누락 시 null 역참조 가능성이 있습니다. |

## 게시 범위

게임 스크립트 13개, 해당 메타데이터, 패키지 정보와 에디터 버전만 발췌했습니다. 원본의 제3자 에셋과 데모 코드는 재배포 범위를 확인하지 못해 제외했습니다. 전체 실행 프로젝트 또는 완성도 검증을 마친 릴리스로 제시하지 않습니다.
