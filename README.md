import json
import os
from datetime import datetime

# 檔案名稱
DATA_FILE = 'wrong_questions.json'

def load_data():
    """載入現有的錯題資料，如果檔案不存在則返回空列表。"""
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, 'r', encoding='utf-8') as f:
            return json.load(f)
    return []

def save_data(data):
    """將錯題資料儲存到 JSON 檔案中。"""
    with open(DATA_FILE, 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=False, indent=4)
    print("✅ 資料已儲存。")

def add_question(data):
    """新增一條錯題記錄。"""
    print("\n--- 新增錯題 ---")
    
    question = input("請輸入題目內容/描述: ")
    answer = input("請輸入正確答案: ")
    subject = input("請輸入科目 (例如: 數學, 英文): ")
    topic = input("請輸入章節/單元: ")
    reason = input("請輸入錯誤原因 (例如: 粗心, 觀念不清): ")
    
    new_id = len(data) + 1
    new_entry = {
        'id': new_id,
        'question': question,
        'answer': answer,
        'subject': subject,
        'topic': topic,
        'reason': reason,
        'date_added': datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        'status': '待複習',
        'review_count': 0
    }
    
    data.append(new_entry)
    save_data(data)
    print(f"🎉 錯題 #{new_id} 已成功加入錯題本！")

def display_questions(data):
    """顯示所有錯題，或根據科目篩選。"""
    if not data:
        print("\n錯題本目前是空的。")
        return

    print("\n--- 錯題列表 ---")
    
    # 讓使用者篩選
    filter_subject = input("輸入要篩選的科目 (留空則顯示全部): ").strip()
    
    filtered_data = [
        q for q in data 
        if not filter_subject or q['subject'].lower() == filter_subject.lower()
    ]
    
    if not filtered_data:
        print(f"找不到科目 '{filter_subject}' 的錯題。")
        return

    for q in filtered_data:
        print("-" * 30)
        print(f"ID: {q['id']}")
        print(f"科目: {q['subject']} | 單元: {q['topic']}")
        print(f"錯誤原因: {q['reason']} | 狀態: {q['status']}")
        print(f"題目: {q['question']}")
        print(f"答案: {q['answer']}")
        print(f"加入時間: {q['date_added']}")
        print(f"已複習次數: {q['review_count']}")
    print("-" * 30)
    print(f"總計 {len(filtered_data)} 條錯題顯示。")

def main():
    """程式主迴圈。"""
    wrong_questions = load_data()
    
    while True:
        print("\n==== 錯題本點子版 ====")
        print("1. 新增錯題")
        print("2. 顯示錯題列表")
        print("3. 結束程式")
        
        choice = input("請輸入您的選擇 (1-3): ")
        
        if choice == '1':
            add_question(wrong_questions)
        elif choice == '2':
            display_questions(wrong_questions)
        elif choice == '3':
            print("程式結束，感謝使用錯題本！")
            break
        else:
            print("⚠️ 無效的選擇，請重新輸入。")

if __name__ == "__main__":
    main()
