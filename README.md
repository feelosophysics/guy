# 🎯 나만의 퀴즈 게임 - 완성 초안 가이드

안녕하세요! 이 가이드는 **처음부터 끝까지 따라하면 과제가 완성**되도록 작성했습니다.
초심자 눈높이에 맞춰 "왜 이렇게 하는지"를 매 단계마다 설명합니다.

---

## 📌 전체 로드맵 (큰 그림 먼저!)

```
Step 1: 개발 환경 확인
Step 2: GitHub 저장소 생성 & 로컬 설정
Step 3: .gitignore, README.md 초안 작성 → 첫 커밋
Step 4: Quiz 클래스 작성 → 커밋
Step 5: 기본 퀴즈 데이터 5개 작성 → 커밋
Step 6: QuizGame 클래스 + 메뉴 기능 → 커밋
Step 7: 퀴즈 풀기 (브랜치 생성 → 작업 → 병합) → 커밋
Step 8: 퀴즈 추가 기능 → 커밋
Step 9: 퀴즈 목록 기능 → 커밋
Step 10: 점수 확인 기능 → 커밋
Step 11: 파일 저장/불러오기 (state.json) → 커밋
Step 12: README.md 최종 작성 → 커밋 & 푸시
Step 13: clone & pull 실습
```

---

## Step 1: 개발 환경 확인

터미널(또는 VSCode 터미널)에서 아래를 확인하세요.

```bash
python --version    # Python 3.10 이상이면 OK
# 만약 python이 안 되면 python3 --version 시도

git --version       # Git 2.x 이상이면 OK
```

> 💡 **왜 확인하나요?**
> 이 과제는 Python 3.10+ 문법과 Git을 사용합니다.
> 버전이 안 맞으면 나중에 에러가 나서 원인 찾기가 어렵습니다.

---

## Step 2: GitHub 저장소 생성 & 로컬 설정

### 2-1. GitHub에서 저장소 만들기

1. GitHub.com → 우측 상단 `+` → `New repository`
2. Repository name: `quiz-game`
3. **Public** 선택
4. "Add a README file" 체크 **하지 마세요** (로컬에서 직접 만들 겁니다)
5. `Create repository` 클릭

### 2-2. 로컬에 프로젝트 폴더 만들기

```bash
# 원하는 위치에 폴더 생성
mkdir quiz-game
cd quiz-game

# Git 저장소 초기화
git init
```

> 💡 **`git init`이 뭔가요?**
> 이 폴더를 "Git이 변경사항을 추적하는 폴더"로 만드는 명령어입니다.
> 실행하면 숨겨진 `.git` 폴더가 생깁니다. 이 안에 모든 변경 이력이 저장됩니다.

### 2-3. GitHub 원격 저장소 연결

```bash
git remote add origin https://github.com/본인아이디/quiz-game.git
```

> 💡 **`remote add origin`이 뭔가요?**
> "이 로컬 폴더의 변경사항을 올릴 원격(remote) 주소를 `origin`이라는 이름으로 등록해줘"라는 뜻입니다.

---

## Step 3: .gitignore, README.md 초안 → 첫 커밋

### 3-1. `.gitignore` 파일 생성

```
# .gitignore
# Python이 자동 생성하는 파일/폴더를 Git에서 제외

__pycache__/
*.pyc
.vscode/
.idea/
```

> 💡 **`.gitignore`가 왜 필요한가요?**
> Python을 실행하면 `__pycache__` 같은 임시 파일이 자동 생성됩니다.
> 이런 파일은 코드가 아니라 "컴퓨터가 만든 캐시"이므로 Git에 올릴 필요가 없습니다.

### 3-2. `README.md` 초안 생성

```markdown
# 🎯 나만의 퀴즈 게임

Python으로 만든 터미널 기반 퀴즈 게임입니다.

## 개발 중...
```

### 3-3. 첫 번째 커밋 & 푸시

```bash
git add .
git commit -m "Init: 프로젝트 초기 설정 (.gitignore, README.md)"
git branch -M main
git push -u origin main
```

> 💡 **명령어 하나씩 뜯어보기:**
> - `git add .` → 현재 폴더의 모든 변경된 파일을 "스테이지"에 올림 (커밋 준비)
> - `git commit -m "메시지"` → 스테이지에 올린 파일들을 하나의 "스냅샷"으로 저장
> - `git branch -M main` → 기본 브랜치 이름을 `main`으로 설정
> - `git push -u origin main` → 로컬 커밋을 GitHub에 업로드
> - `-u`는 "앞으로 `git push`만 쳐도 origin main으로 보내줘"라는 설정

---

## Step 4: Quiz 클래스 작성

이제 본격적으로 코드를 작성합니다! `main.py` 파일을 만드세요.

### 4-1. Quiz 클래스 코드

```python
# main.py

class Quiz:
    """
    개별 퀴즈 하나를 표현하는 클래스
    
    왜 클래스를 쓰나요?
    → 퀴즈 하나에는 "문제", "선택지 4개", "정답"이 항상 함께 다닙니다.
    → 이렇게 관련 있는 데이터를 하나로 묶어두면 관리가 편합니다.
    → 마치 "퀴즈 카드" 한 장을 만드는 것과 같습니다.
    """
    
    def __init__(self, question, choices, answer):
        """
        Quiz 객체를 생성할 때 자동으로 호출되는 초기화 메서드
        
        매개변수(Parameters):
            question (str): 퀴즈 문제 텍스트
                예: "Python의 창시자는?"
            choices (list): 선택지 4개가 담긴 리스트
                예: ["Guido", "Linus", "Bjarne", "James"]
            answer (int): 정답 번호 (1~4)
                예: 1 (첫 번째 선택지가 정답)
        
        왜 self를 쓰나요?
        → self는 "이 객체 자신"을 가리킵니다.
        → self.question = question 은
          "이 퀴즈 카드의 question 칸에 전달받은 question을 적어라"라는 뜻입니다.
        """
        self.question = question
        self.choices = choices
        self.answer = answer
    
    def display(self, number):
        """
        퀴즈를 화면에 예쁘게 출력하는 메서드
        
        매개변수:
            number (int): 몇 번째 문제인지 (예: 1, 2, 3...)
        
        반환값: 없음 (화면에 출력만 함)
        """
        print(f"\n[문제 {number}]")
        print(f"{self.question}\n")
        
        # enumerate를 쓰면 인덱스(i)와 값(choice)을 동시에 가져올 수 있습니다
        # i는 0부터 시작하므로 i+1로 1부터 표시합니다
        for i, choice in enumerate(self.choices):
            print(f"  {i + 1}. {choice}")
        print()  # 빈 줄 하나 추가 (가독성)
    
    def check_answer(self, user_input):
        """
        사용자가 입력한 답이 정답인지 확인하는 메서드
        
        매개변수:
            user_input (int): 사용자가 입력한 번호 (1~4)
        
        반환값:
            bool: 정답이면 True, 오답이면 False
        
        왜 메서드로 분리하나요?
        → "정답 확인"이라는 동작을 한 곳에서 관리하면
          나중에 로직을 바꿀 때 이 메서드만 수정하면 됩니다.
        """
        return user_input == self.answer
    
    def to_dict(self):
        """
        Quiz 객체를 딕셔너리(dict)로 변환하는 메서드
        
        왜 필요한가요?
        → JSON 파일에 저장하려면 Python 객체를 dict로 바꿔야 합니다.
        → JSON은 클래스 객체를 직접 저장할 수 없거든요.
        
        반환값:
            dict: {"question": "...", "choices": [...], "answer": N}
        """
        return {
            "question": self.question,
            "choices": self.choices,
            "answer": self.answer
        }
    
    @classmethod
    def from_dict(cls, data):
        """
        딕셔너리에서 Quiz 객체를 생성하는 클래스 메서드
        
        왜 필요한가요?
        → JSON 파일에서 데이터를 읽으면 dict 형태입니다.
        → 이 dict를 다시 Quiz 객체로 변환해야 프로그램에서 사용할 수 있습니다.
        
        @classmethod란?
        → 일반 메서드는 이미 만들어진 객체(self)에서 호출하지만,
          클래스 메서드는 클래스(cls) 자체에서 호출하여 새 객체를 만들 수 있습니다.
        → Quiz.from_dict(data) 형태로 사용합니다.
        
        매개변수:
            data (dict): JSON에서 읽어온 퀴즈 데이터
        
        반환값:
            Quiz: 새로 생성된 Quiz 객체
        """
        return cls(
            question=data["question"],
            choices=data["choices"],
            answer=data["answer"]
        )
```

> 💡 **초심자를 위한 핵심 정리:**
> ```python
> # 클래스 = 설계도, 객체 = 설계도로 만든 실제 물건
> # Quiz 클래스 = "퀴즈 카드 양식"
> # Quiz("문제", ["1","2","3","4"], 1) = "실제 퀴즈 카드 한 장"
> 
> q = Quiz("1+1은?", ["1", "2", "3", "4"], 2)
> q.display(1)        # 화면에 출력
> q.check_answer(2)   # True (정답!)
> q.check_answer(3)   # False (오답!)
> ```

### 4-2. 커밋

```bash
git add main.py
git commit -m "Feat: Quiz 클래스 정의 (문제/선택지/정답 관리)"
git push
```

---

## Step 5: 기본 퀴즈 데이터 5개 작성

주제: **영화 상식** (마블, SF, 한국영화 등)

`main.py`에 아래 함수를 **Quiz 클래스 아래에** 추가합니다.

```python
def get_default_quizzes():
    """
    기본 퀴즈 데이터를 반환하는 함수
    
    왜 함수로 분리하나요?
    → state.json 파일이 없거나 손상되었을 때 이 기본 데이터를 사용합니다.
    → 한 곳에서 관리하면 기본 퀴즈를 수정할 때 편합니다.
    
    반환값:
        list: Quiz 객체 5개가 담긴 리스트
    """
    return [
        Quiz(
            question="마블 시네마틱 유니버스에서 타노스가 모은 인피니티 스톤의 개수는?",
            choices=["4개", "5개", "6개", "7개"],
            answer=3  # 6개가 정답
        ),
        Quiz(
            question="영화 '인터스텔라'에서 주인공 쿠퍼의 직업은?",
            choices=["과학자", "전직 파일럿/농부", "의사", "군인"],
            answer=2
        ),
        Quiz(
            question="영화 '기생충'의 감독은 누구인가?",
            choices=["박찬욱", "봉준호", "김기덕", "이창동"],
            answer=2
        ),
        Quiz(
            question="영화 '매트릭스'에서 주인공 네오가 선택한 알약의 색깔은?",
            choices=["파란색", "초록색", "빨간색", "노란색"],
            answer=3
        ),
        Quiz(
            question="스타워즈 시리즈에서 '제다이'가 사용하는 무기의 이름은?",
            choices=["블래스터", "광선검(라이트세이버)", "포스건", "플
