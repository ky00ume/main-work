# 비전 타운 RPG 봇 — Claude Desktop 작업 가이드

## 프로젝트 개요
- **프로젝트명**: 비전 타운 RPG 디스코드 봇
- **스택**: Python, discord.py>=2.0.0, pytz, python-dotenv, Pillow
- **실행**: `python main.py`
- **환경 변수**: `.env` 파일에 `DISCORD_TOKEN` 등 설정

## 빌드 및 실행 명령어

```bash
# 의존성 설치
pip install -r requirements.txt

# 봇 실행
python main.py

# 단일 모듈 테스트 (터미널 없이 Claude Desktop에서 코드 검토용)
# python -c "from battle import BattleEngine; ..."
```

## 코드베이스 아키텍처

### 핵심 모듈
- `main.py` — 봇 진입점, 모든 커맨드 등록 (111KB, 가장 큰 파일)
- `player.py` — Player 클래스, 플레이어 스탯·데이터 관리
- `database.py` — SQLite DB 연동, `get_db_connection()` 사용
- `save_manager.py` — 세이브/로드: `save_manager.save(shared_player)`

### 콘텐츠 DB 파일
- `items.py` — 전체 아이템 정의 (72KB)
- `monsters_db.py` — 몬스터 DB
- `skills_db.py` — 스킬 DB
- `cooking_db.py` — 요리 레시피 DB
- `job_data.py` — 직업 데이터 (48KB)
- `npc_dialogue_db.py` — NPC 대사 DB (131KB)
- `adventure_data.py` — 모험 이벤트 데이터
- `story_quest_data.py` — 스토리 퀘스트 데이터
- `costume_data.py` — 코스튬 데이터

### 시스템 모듈
- `battle.py` — 전투 엔진
- `battle_view.py` / `battle_event_data.py` / `battle_log_data.py` — 전투 연관
- `fishing.py` / `fishing_card.py` — 낚시 시스템
- `crafting.py` — 제작 시스템
- `metallurgy.py` — 제련 시스템
- `gathering.py` / `gather_bridge.py` — 채집 시스템
- `quest.py` / `quest_ui.py` / `quest_bridge.py` — 퀘스트 시스템
- `story_quest.py` / `story_quest_ui.py` — 스토리 퀘스트
- `npcs.py` / `npc_conversation.py` — NPC 시스템
- `special_npc.py` / `special_npc_ui.py` — 특수 NPC
- `shop.py` / `shop_ui.py` — 상점
- `adventure.py` / `adventure_npc_data.py` — 모험
- `achievements.py` — 업적
- `affinity.py` — 친밀도
- `care.py` / `care_ui.py` — 케어 시스템
- `gacha.py` — 가챠
- `economy.py` — 경제 시스템
- `training.py` — 훈련
- `rest.py` — 휴식
- `restaurant.py` — 식당
- `storage.py` / `storage_ui.py` — 창고
- `village.py` — 마을
- `town_ui.py` / `town_notice.py` — 마을 UI·공지
- `movement.py` — 이동
- `weather.py` — 날씨
- `alarms.py` — 알림
- `diary.py` — 일기
- `bulletin.py` — 게시판
- `collection.py` — 컬렉션
- `music.py` — 음악
- `potion.py` — 포션
- `responses.py` — 응답 처리
- `transaction_log.py` — 거래 기록

### UI 모듈
- `bg3_renderer.py` — 렌더러 (52KB)
- `ui_theme.py` — UI 테마
- `skill_ui.py` — 스킬 UI
- `status.py` / `status_window.py` — 상태창
- `equipment_window.py` — 장비창

## 봇 패턴 및 컨벤션

### discord.py 커맨드 패턴
```python
# 모든 커맨드는 @bot.command 데코레이터 사용
@bot.command(name="커맨드명")
async def command_name(ctx, *args):
    await ctx.send("응답 메시지")
```

### DB 패턴
```python
# DB 연결
conn = get_db_connection()
cursor = conn.cursor()
# 작업 후 반드시 close
conn.close()
```

### 세이브 패턴
```python
save_manager.save(shared_player)
```

### 코딩 컨벤션
- 커맨드명은 한국어 사용 가능
- 출력은 ANSI 스타일 임베드 활용
- 비동기 처리: `async/await` 필수

## 새 기능 추가 시 체크리스트
1. `items.py` 또는 관련 DB 파일에 데이터 추가
2. 로직 모듈 파일 생성 또는 수정
3. UI 파일 생성 또는 수정 (필요 시)
4. `main.py`에 커맨드 등록
5. `database.py`에 DB 스키마 추가 (필요 시)

## Claude Desktop 사용 팁
- 이 파일을 새 대화 시작 시 항상 첨부하세요
- 큰 파일(main.py, items.py 등)은 수정할 함수/섹션만 잘라서 붙여넣으세요
- 터미널 없이 작업하므로, 실행 테스트는 직접 봇을 실행해서 확인하세요
