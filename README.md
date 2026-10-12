# Car Booking App - bản mô phỏng đơn giản

drivers = [
    {"name": "Tài xế An", "status": "Rảnh"},
    {"name": "Tài xế Bình", "status": "Rảnh"},
    {"name": "Tài xế Chi", "status": "Rảnh"},
]

bookings = []


def dat_xe():
    customer = input("Tên khách hàng: ").strip()
    pickup = input("Điểm đón: ").strip()
    dropoff = input("Điểm đến: ").strip()

    if not customer or not pickup or not dropoff:
        print("Vui lòng nhập đầy đủ thông tin.\n")
        return

    driver = None
    for item in drivers:
        if item["status"] == "Rảnh":
            driver = item
            break

    if driver is None:
        print("Hiện không có tài xế rảnh.\n")
        return

    booking = {
        "id": len(bookings) + 1,
        "customer": customer,
        "pickup": pickup,
        "dropoff": dropoff,
        "driver": driver["name"],
        "status": "Đã nhận",
        "payment": "Chưa thanh toán",
        "rating": None,
    }

    bookings.append(booking)
    driver["status"] = "Đang bận"

    print(f"Đặt xe thành công. Tài xế: {driver['name']}")
    print(f"Mã chuyến: {booking['id']}\n")


def xem_chuyen():
    if not bookings:
        print("Chưa có chuyến xe nào.\n")
        return

    print("\n--- Danh sách chuyến xe ---")
    for booking in bookings:
        print(
            f"Mã: {booking['id']} | "
            f"Khách: {booking['customer']} | "
            f"{booking['pickup']} -> {booking['dropoff']} | "
            f"Tài xế: {booking['driver']} | "
            f"Trạng thái: {booking['status']} | "
            f"Thanh toán: {booking['payment']} | "
            f"Đánh giá: {booking['rating'] or 'Chưa đánh giá'}"
        )
    print()


def tim_chuyen_theo_ma():
    try:
        booking_id = int(input("Nhập mã chuyến: "))
    except ValueError:
        print("Mã chuyến phải là số.\n")
        return None

    for booking in bookings:
        if booking["id"] == booking_id:
            return booking

    print("Không tìm thấy chuyến xe.\n")
    return None


def cap_nhat_chuyen():
    booking = tim_chuyen_theo_ma()
    if booking is None:
        return

    next_status = {
        "Đã nhận": "Tài xế đang đến",
        "Tài xế đang đến": "Đang di chuyển",
        "Đang di chuyển": "Hoàn thành",
    }

    if booking["status"] not in next_status:
        print("Chuyến xe đã hoàn thành, không thể cập nhật thêm.\n")
        return

    booking["status"] = next_status[booking["status"]]
    print(f"Trạng thái chuyến: {booking['status']}\n")

    if booking["status"] == "Hoàn thành":
        for driver in drivers:
            if driver["name"] == booking["driver"]:
                driver["status"] = "Rảnh"
                break


def thanh_toan():
    booking = tim_chuyen_theo_ma()
    if booking is None:
        return

    if booking["status"] != "Hoàn thành":
        print("Chỉ thanh toán sau khi chuyến xe hoàn thành.\n")
        return

    if booking["payment"] != "Chưa thanh toán":
        print("Chuyến xe này đã được thanh toán.\n")
        return

    print("1. Tiền mặt")
    print("2. QR")
    choice = input("Chọn phương thức thanh toán: ").strip()

    if choice == "1":
        booking["payment"] = "Tiền mặt"
    elif choice == "2":
        booking["payment"] = "QR"
    else:
        print("Lựa chọn không hợp lệ.\n")
        return

    print("Thanh toán thành công (mô phỏng).\n")


def danh_gia():
    booking = tim_chuyen_theo_ma()
    if booking is None:
        return

    if booking["status"] != "Hoàn thành":
        print("Chỉ đánh giá sau khi chuyến xe hoàn thành.\n")
        return

    if booking["rating"] is not None:
        print("Chuyến xe này đã được đánh giá.\n")
        return

    try:
        rating = int(input("Đánh giá tài xế từ 1 đến 5 sao: "))
    except ValueError:
        print("Điểm đánh giá phải là số.\n")
        return

    if rating < 1 or rating > 5:
        print("Điểm đánh giá phải từ 1 đến 5.\n")
        return

    booking["rating"] = rating
    print("Cảm ơn bạn đã đánh giá!\n")


while True:
    print("=== CAR BOOKING APP ===")
    print("1. Đặt xe")
    print("2. Cập nhật trạng thái chuyến")
    print("3. Thanh toán")
    print("4. Đánh giá tài xế")
    print("5. Xem danh sách chuyến")
    print("0. Thoát")

    choice = input("Chọn chức năng: ").strip()

    if choice == "1":
        dat_xe()
    elif choice == "2":
        cap_nhat_chuyen()
    elif choice == "3":
        thanh_toan()
    elif choice == "4":
        danh_gia()
    elif choice == "5":
        xem_chuyen()
    elif choice == "0":
        print("Đã thoát chương trình.")
        break
    else:
        print("Lựa chọn không hợp lệ.\n")
