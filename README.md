# CCVC - Quan ly Cong van va Dieu hanh Cong viec

Bo cong cu va kich ban tu dong hoa quan ly cong van den/di tren Cong thong tin Chinh quyen dien tu tinh Quang Ninh (congchuc.quangninh.gov.vn).

## Tinh nang chinh
- Tra cuu va quet truc tiep danh sach cong van den (list, list web)
- Ket thuc xu ly don le va Native Bulk End hang loat 50 van ban trong 10-15s
- Tai file dinh kem va tom tat noi dung van ban bang AI
- Soan thao du thao va hieu dinh van phong chuan Nghi dinh 30/2020/ND-CP
- Tra cuu va theo doi van ban di

## Cau truc thu muc
- SKILL.md: Huong dan danh cho AI Agent
- scripts/: Cac kich ban Python thao tac cong va xu ly van ban
- lib/: Router va inline payload
- references/: Tai lieu ky thuat va troubleshooting
- templates/: Mau van ban hanh chinh

## Cau hinh .env
CONGVAN_USER=tai_khoan_sso
CONGVAN_PASS=mat_khau_sso
CONGVAN_URL=https://congchuc.quangninh.gov.vn/Default.aspx?tabid=1126
CONGVAN_UNIT=2256,226
