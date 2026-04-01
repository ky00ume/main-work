# 컨텍스트 프라임 — 비전 타운 RPG 봇

이 대화에서 작업할 프로젝트의 전체 컨텍스트를 먼저 파악해줘.

## 프로젝트 정보
- **프로젝트명**: 비전 타운 RPG 디스코드 봇
- **레포**: ky00ume/BOT
- **스택**: Python, discord.py>=2.0.0, SQLite, pytz, python-dotenv, Pillow
- **실행**: `python main.py`

## 핵심 구조
- `main.py` — 봇 진입점 + 모든 커맨드 등록 (가장 큰 파일, 111KB)
- `player.py` — Player 클래스 (스탯, 인벤토리, 상태)
- `battle.py` — 전투 엔진
- `database.py` — SQLite 연결 (`get_db_connection()`)
- `save_manager.py` — 세이브/로드 (`save_manager.save(shared_player)`)
- `items.py` — 전체 아이템 정의 (72KB)
- `npc_dialogue_db.py` — NPC 대사 DB (131KB)

## 봇 커맨드 패턴
```python
@bot.command(name="커맨드명")
async def command_name(ctx, *args):
    await ctx.send("응답")
```

## 작업 제약
- 터미널 사용 불가 — Claude Desktop에서만 작업
- 코드 수정 후 직접 봇 실행으로 테스트
- 한국어 커맨드명 사용

## 이번 작업 내용
<!-- 여기에 오늘 작업할 내용을 적어서 사용하세요 -->
[작업 내용을 여기에 입력]

위 컨텍스트를 바탕으로, 내가 첨부하는 코드와 질문에 답해줘.
