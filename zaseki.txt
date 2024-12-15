import streamlit as st
import random


if 'available_seats' not in st.session_state:
    st.session_state.available_seats = list(range(1, 6))  # 席番号1〜5
if 'assigned_seats' not in st.session_state:
    st.session_state.assigned_seats = {}

def assign_random_seat(name):
    if not st.session_state.available_seats:
        return None
    seat_number = random.choice(st.session_state.available_seats)
    st.session_state.available_seats.remove(seat_number)
    st.session_state.assigned_seats[name] = seat_number
    return seat_number

def main():
    st.title("出席確認システム")
    
    if len(st.session_state.assigned_seats) >= 5:
        st.warning("席は埋まりました。申し訳ありませんが、後ほどお試しください。")
        return
    
    name = st.text_input("名前を入力してください")
    
    if st.button("登録"):
        if not name.strip():
            st.error("名前を入力してください。")
        elif name in st.session_state.assigned_seats:
            seat_number = st.session_state.assigned_seats[name]
            st.info(f"{name}さんは既に席番号 {seat_number} に割り当てられています。")
        else:
            seat_number = assign_random_seat(name)
            if seat_number:
                st.success(f"{name}さんは席番号 {seat_number} です。")
                if len(st.session_state.assigned_seats) == 5:
                    st.warning("席は埋まりました。")
            else:
                st.warning("席は埋まりました。申し訳ありませんが、後ほどお試しください。")
    
    if st.session_state.assigned_seats:
        st.subheader("現在の席状況")
        for person, seat in st.session_state.assigned_seats.items():
            st.write(f"{person}さん: 席番号 {seat}")

if __name__ == "__main__":
    main()
