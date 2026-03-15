# Supabase 설정 가이드

## 1단계: Supabase 프로젝트 생성

1. https://supabase.com 접속 → **Start your project** 클릭
2. GitHub 계정으로 로그인
3. **New project** → 이름 입력 (예: `tennis-members`) → 비밀번호 설정 → Region: **Northeast Asia (Seoul)** → Create project

---

## 2단계: 데이터베이스 테이블 생성

좌측 메뉴 **SQL Editor** 클릭 후 아래 SQL을 붙여넣고 **Run** 클릭:

```sql
-- 회원 테이블 생성
CREATE TABLE members (
  id         uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  name       text NOT NULL,
  dept       text NOT NULL,
  photo_url  text,
  created_at timestamptz DEFAULT now()
);

-- 누구나 읽기/쓰기/삭제 가능하도록 허용 (내부 동아리용)
ALTER TABLE members ENABLE ROW LEVEL SECURITY;

CREATE POLICY "public_all" ON members
  FOR ALL USING (true) WITH CHECK (true);
```

---

## 3단계: Storage 버킷 생성

1. 좌측 메뉴 **Storage** 클릭
2. **New bucket** → 이름: `photos` → **Public bucket 체크** → Create bucket
3. 버킷 생성 후 **Policies** 탭 → **New policy** → "For full customization" → 아래 설정:
   - Policy name: `public_all`
   - Allowed operation: SELECT, INSERT, UPDATE, DELETE 모두 체크
   - Target roles: (비워두기, 전체 허용)
   - USING expression: `true`
   - Save policy

---

## 4단계: API 키 확인

1. 좌측 메뉴 **Project Settings** → **API**
2. 다음 두 값을 복사:
   - **Project URL** → `SUPABASE_URL`에 붙여넣기
   - **anon / public** 키 → `SUPABASE_ANON_KEY`에 붙여넣기

---

## 5단계: index.html 수정

`index.html` 하단 `<script>` 블록 상단을 수정:

```javascript
const SUPABASE_URL      = 'https://abcdefghij.supabase.co';  // ← 본인 URL
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6...'; // ← 본인 키
```

수정 후 `#config-banner` div 블록도 삭제하세요.

---

## 6단계: GitHub에 push

```bash
git add index.html
git commit -m "Supabase 연동"
git push
```

GitHub Pages가 활성화되어 있으면 자동으로 반영됩니다.

---

## GitHub Pages 활성화 (아직 안 했다면)

1. 저장소 → **Settings** → **Pages**
2. Source: **Deploy from a branch** → Branch: `main` → `/ (root)` → Save
3. 잠시 후 `https://reihyoungai-web.github.io/tennis-members/` 로 접근 가능
