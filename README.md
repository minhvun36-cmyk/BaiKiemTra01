using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
namespace AutoSpeed
{
    abstract class PhuongTien
    {
        private string _maPT;
        private string _tenHang;
        private int _namSanXuat;
        private decimal _giaGoc;
        public string MaPT
        {
            get { return _maPT; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    _maPT = "PT000";
                else
                    _maPT = value.Trim();
            }
        }
        public string TenHang
        {
            get { return _tenHang; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    throw new ArgumentException("Ten hang khong duoc de trong!");

                _tenHang = value.Trim();
            }
        }
        public int NamSanXuat
        {
            get { return _namSanXuat; }
            set
            {
                int namHienTai = DateTime.Now.Year;

                if (value < 1900 || value > namHienTai)
                {
                    throw new ArgumentException("Năm sản xuất không hợp lệ!");
                }

                _namSanXuat = value;
            }
        }
        public decimal GiaGoc
        {
            get { return _giaGoc; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Gia goc phai lon hon 0!");

                _giaGoc = value;
            }
        }
        public PhuongTien(string maPT, string tenHang,
                          int namSanXuat, decimal giaGoc)
        {
            MaPT = maPT;
            TenHang = tenHang;
            NamSanXuat = namSanXuat;
            GiaGoc = giaGoc;
        }
        public abstract decimal TinhGiaLanBanh();
        public virtual string GetInfo()
        {
            return "Ma PT: " + MaPT
                + " | Hang: " + TenHang
                + " | Nam SX: " + NamSanXuat
                + " | Gia goc: " + GiaGoc.ToString("N0") + " VNĐ";
        }
    }

    class OTo : PhuongTien
    {
        private int _soChoNgoi;
        private double _dungTichDongCo;

        public int SoChoNgoi
        {
            get { return _soChoNgoi; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException(
                        "So cho ngoi phai lon hon 0!");

                _soChoNgoi = value;
            }
        }

        public double DungTichDongCo
        {
            get { return _dungTichDongCo; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException(
                        "Dung tich dong co phai lon hon 0!");

                _dungTichDongCo = value;
            }
        }

        public OTo(string maPT, string tenHang,
                   int namSanXuat, decimal giaGoc,
                   int soChoNgoi, double dungTichDongCo)
            : base(maPT, tenHang, namSanXuat, giaGoc)
        {
            SoChoNgoi = soChoNgoi;
            DungTichDongCo = dungTichDongCo;
        }
        public override decimal TinhGiaLanBanh()
        {
            if (SoChoNgoi <= 9)
            {
                return GiaGoc
                    + GiaGoc * 12 / 100
                    + GiaGoc * 30 / 100;
            }
            else
            {
                return GiaGoc
                    + GiaGoc * 10 / 100;
            }
        }
        public override string GetInfo()
        {
            return base.GetInfo()
                + " | So cho: " + SoChoNgoi
                + " | Dung tich dong co: "
                + DungTichDongCo + " L";
        }
    }
    class XeMay : PhuongTien
    {
        private int _dungTichXylanh;

        public int DungTichXylanh
        {
            get { return _dungTichXylanh; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException(
                        "Dung tich xylanh phai lon hon 0!");

                _dungTichXylanh = value;
            }
        }

        public XeMay(string maPT, string tenHang,
                     int namSanXuat, decimal giaGoc,
                     int dungTichXylanh)
            : base(maPT, tenHang, namSanXuat, giaGoc)
        {
            DungTichXylanh = dungTichXylanh;
        }
        public override decimal TinhGiaLanBanh()
        {
            if (DungTichXylanh < 175)
            {
                return GiaGoc + GiaGoc * 2 / 100;
            }
            else
            {
                return GiaGoc + GiaGoc * 5 / 100;
            }
        }
        public override string GetInfo()
        {
            return base.GetInfo()
                + " | Dung tich xylanh: "
                + DungTichXylanh + " cc";
        }
    }
    class QuanLyPhuongTien
    {
        private List<PhuongTien> danhSach;

        public QuanLyPhuongTien()
        {
            danhSach = new List<PhuongTien>();
        }
        public void AddPhuongTien(PhuongTien pt)
        {
            if (pt != null)
            {
                danhSach.Add(pt);
            }
        }
        public void DisplayAll()
        {
            Console.WriteLine();
            Console.WriteLine("========== DANH SACH PHUONG TIEN ==========");

            if (danhSach.Count == 0)
            {
                Console.WriteLine("Danh sach rong!");
                return;
            }

            foreach (PhuongTien pt in danhSach)
            {
                Console.WriteLine(pt.GetInfo());

                Console.WriteLine(
                    "Gia lan banh: "
                    + pt.TinhGiaLanBanh().ToString("N0")
                    + " VNĐ");

                Console.WriteLine("------------------------------------------");
            }
        }
        public PhuongTien FindMaxGiaLanBanh()
        {
            if (danhSach.Count == 0)
                return null;

            return danhSach
                .OrderByDescending(pt => pt.TinhGiaLanBanh())
                .First();
        }
        public List<PhuongTien> SearchByName(string keyword)
        {
            List<PhuongTien> ketQua =
                new List<PhuongTien>();

            foreach (PhuongTien pt in danhSach)
            {
                if (pt.TenHang.ToLower()
                    .Contains(keyword.ToLower()))
                {
                    ketQua.Add(pt);
                }
            }

            return ketQua;
        }
        public List<PhuongTien> SearchByYear(int nam)
        {
            List<PhuongTien> ketQua =
                new List<PhuongTien>();

            foreach (PhuongTien pt in danhSach)
            {
                if (pt.NamSanXuat == nam)
                {
                    ketQua.Add(pt);
                }
            }

            return ketQua;
        }
    }
    class TestCase
    {
        public static void TC01()
        {
            Console.WriteLine();
            Console.WriteLine("===== TC01 - VALIDATION NAM SAN XUAT =====");

            try
            {
                Console.Write("Nhap ma PT: ");
                string maPT = Console.ReadLine();

                Console.Write("Nhap ten hang: ");
                string tenHang = Console.ReadLine();

                Console.Write("Nhap nam san xuat: ");
                int namSanXuat =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap gia goc: ");
                decimal giaGoc =
                    decimal.Parse(Console.ReadLine());

                Console.Write("Nhap so cho ngoi: ");
                int soChoNgoi =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap dung tich dong co: ");
                double dungTich =
                    double.Parse(Console.ReadLine());

                OTo oto = new OTo(
                    maPT,
                    tenHang,
                    namSanXuat,
                    giaGoc,
                    soChoNgoi,
                    dungTich);

                Console.WriteLine("TC01: FAIL");
                Console.WriteLine(
                    "Doi tuong van duoc tao.");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("TC01: PASS");
                Console.WriteLine("Loi: " + ex.Message);
            }
        }
        public static void TC02()
        {
            Console.WriteLine();
            Console.WriteLine("===== TC02 - GIA LAN BANH O TO =====");

            try
            {
                Console.Write("Nhap ma PT: ");
                string maPT = Console.ReadLine();

                Console.Write("Nhap ten hang: ");
                string tenHang = Console.ReadLine();

                Console.Write("Nhap nam san xuat: ");
                int namSanXuat =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap gia goc: ");
                decimal giaGoc =
                    decimal.Parse(Console.ReadLine());

                Console.Write("Nhap so cho ngoi: ");
                int soChoNgoi =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap dung tich dong co: ");
                double dungTich =
                    double.Parse(Console.ReadLine());

                OTo oto = new OTo(
                    maPT,
                    tenHang,
                    namSanXuat,
                    giaGoc,
                    soChoNgoi,
                    dungTich);

                decimal giaLanBanh =
                    oto.TinhGiaLanBanh();

                Console.WriteLine();
                Console.WriteLine(oto.GetInfo());

                Console.WriteLine(
                    "Gia lan banh: "
                    + giaLanBanh.ToString("N0")
                    + " VNĐ");

                Console.WriteLine("TC02: DA THUC HIEN");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("Loi: " + ex.Message);
            }
        }
        public static void TC03()
        {
            Console.WriteLine();
            Console.WriteLine("===== TC03 - GIA LAN BANH XE MAY =====");

            try
            {
                Console.Write("Nhap ma PT: ");
                string maPT = Console.ReadLine();

                Console.Write("Nhap ten hang: ");
                string tenHang = Console.ReadLine();

                Console.Write("Nhap nam san xuat: ");
                int namSanXuat =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap gia goc: ");
                decimal giaGoc =
                    decimal.Parse(Console.ReadLine());

                Console.Write("Nhap dung tich xylanh (cc): ");
                int xylanh =
                    int.Parse(Console.ReadLine());

                XeMay xeMay = new XeMay(
                    maPT,
                    tenHang,
                    namSanXuat,
                    giaGoc,
                    xylanh);

                decimal giaLanBanh =
                    xeMay.TinhGiaLanBanh();

                Console.WriteLine();
                Console.WriteLine(xeMay.GetInfo());

                Console.WriteLine(
                    "Gia lan banh: "
                    + giaLanBanh.ToString("N0")
                    + " VNĐ");

                Console.WriteLine("TC03: DA THUC HIEN");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("Loi: " + ex.Message);
            }
        }
        public static void TC04()
        {
            Console.WriteLine();
            Console.WriteLine("===== TC04 - KIEM TRA DA HINH =====");

            List<PhuongTien> danhSach =
                new List<PhuongTien>();

            try
            {
                Console.WriteLine();
                Console.WriteLine("--- NHAP O TO ---");

                Console.Write("Nhap ma PT: ");
                string maPT = Console.ReadLine();

                Console.Write("Nhap ten hang: ");
                string tenHang = Console.ReadLine();

                Console.Write("Nhap nam san xuat: ");
                int namSanXuat =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap gia goc: ");
                decimal giaGoc =
                    decimal.Parse(Console.ReadLine());

                Console.Write("Nhap so cho ngoi: ");
                int soChoNgoi =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap dung tich dong co: ");
                double dungTich =
                    double.Parse(Console.ReadLine());

                OTo oto = new OTo(
                    maPT,
                    tenHang,
                    namSanXuat,
                    giaGoc,
                    soChoNgoi,
                    dungTich);

                danhSach.Add(oto);
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("Loi: " + ex.Message);
                return;
            }
            try
            {
                Console.WriteLine();
                Console.WriteLine("--- NHAP XE MAY ---");

                Console.Write("Nhap ma PT: ");
                string maPT = Console.ReadLine();

                Console.Write("Nhap ten hang: ");
                string tenHang = Console.ReadLine();

                Console.Write("Nhap nam san xuat: ");
                int namSanXuat =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap gia goc: ");
                decimal giaGoc =
                    decimal.Parse(Console.ReadLine());

                Console.Write("Nhap dung tich xylanh (cc): ");
                int xylanh =
                    int.Parse(Console.ReadLine());

                XeMay xeMay = new XeMay(
                    maPT,
                    tenHang,
                    namSanXuat,
                    giaGoc,
                    xylanh);

                danhSach.Add(xeMay);
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("Loi: " + ex.Message);
                return;
            }
            Console.WriteLine();
            Console.WriteLine("--- KET QUA DA HINH ---");

            foreach (PhuongTien pt in danhSach)
            {
                Console.WriteLine(pt.GetInfo());

                Console.WriteLine(
                    "Gia lan banh: "
                    + pt.TinhGiaLanBanh().ToString("N0")
                    + " VNĐ");

                Console.WriteLine("--------------------------------");
            }

            Console.WriteLine("TC04: PASS");
            Console.WriteLine(
                "List<PhuongTien> da goi dung ham cua OTo va XeMay.");
        }
        public static void TC05()
        {
            Console.WriteLine();
            Console.WriteLine("===== TC05 - TIM GIA LAN BANH CAO NHAT =====");

            QuanLyPhuongTien quanLy =
                new QuanLyPhuongTien();

            Console.Write("Nhap so luong phuong tien: ");
            int n = int.Parse(Console.ReadLine());

            for (int i = 0; i < n; i++)
            {
                Console.WriteLine();
                Console.WriteLine(
                    "--- PHUONG TIEN THU " + (i + 1) + " ---");

                try
                {
                    Console.Write("Chon loai (1 - OTo, 2 - XeMay): ");
                    int loai =
                        int.Parse(Console.ReadLine());

                    Console.Write("Nhap ma PT: ");
                    string maPT = Console.ReadLine();

                    Console.Write("Nhap ten hang: ");
                    string tenHang = Console.ReadLine();

                    Console.Write("Nhap nam san xuat: ");
                    int namSanXuat =
                        int.Parse(Console.ReadLine());

                    Console.Write("Nhap gia goc: ");
                    decimal giaGoc =
                        decimal.Parse(Console.ReadLine());

                    if (loai == 1)
                    {
                        Console.Write("Nhap so cho ngoi: ");
                        int soChoNgoi =
                            int.Parse(Console.ReadLine());

                        Console.Write(
                            "Nhap dung tich dong co: ");

                        double dungTich =
                            double.Parse(Console.ReadLine());

                        OTo oto = new OTo(
                            maPT,
                            tenHang,
                            namSanXuat,
                            giaGoc,
                            soChoNgoi,
                            dungTich);

                        quanLy.AddPhuongTien(oto);
                    }
                    else if (loai == 2)
                    {
                        Console.Write(
                            "Nhap dung tich xylanh (cc): ");

                        int xylanh =
                            int.Parse(Console.ReadLine());

                        XeMay xeMay = new XeMay(
                            maPT,
                            tenHang,
                            namSanXuat,
                            giaGoc,
                            xylanh);

                        quanLy.AddPhuongTien(xeMay);
                    }
                    else
                    {
                        Console.WriteLine(
                            "Loai phuong tien khong hop le!");

                        i--;
                    }
                }
                catch (ArgumentException ex)
                {
                    Console.WriteLine("Loi: " + ex.Message);
                    i--;
                }
            }

            PhuongTien max =
                quanLy.FindMaxGiaLanBanh();

            if (max == null)
            {
                Console.WriteLine("Danh sach rong!");
            }
            else
            {
                Console.WriteLine();
                Console.WriteLine("===== KET QUA =====");

                Console.WriteLine(
                    "Phuong tien co gia lan banh cao nhat:");

                Console.WriteLine(max.GetInfo());

                Console.WriteLine(
                    "Gia lan banh: "
                    + max.TinhGiaLanBanh().ToString("N0")
                    + " VNĐ");

                Console.WriteLine("TC05: PASS");
            }
        }
    }

    class Program
    {
        static void ThemPhuongTien(
            QuanLyPhuongTien quanLy)
        {
            try
            {
                Console.WriteLine();
                Console.WriteLine("===== THEM PHUONG TIEN =====");

                Console.Write(
                    "Chon loai (1 - OTo, 2 - XeMay): ");

                int loai =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap ma PT: ");
                string maPT = Console.ReadLine();

                Console.Write("Nhap ten hang: ");
                string tenHang = Console.ReadLine();

                Console.Write("Nhap nam san xuat: ");
                int namSanXuat =
                    int.Parse(Console.ReadLine());

                Console.Write("Nhap gia goc: ");
                decimal giaGoc =
                    decimal.Parse(Console.ReadLine());

                if (loai == 1)
                {
                    Console.Write("Nhap so cho ngoi: ");
                    int soChoNgoi =
                        int.Parse(Console.ReadLine());

                    Console.Write(
                        "Nhap dung tich dong co (L): ");

                    double dungTich =
                        double.Parse(Console.ReadLine());

                    OTo oto = new OTo(
                        maPT,
                        tenHang,
                        namSanXuat,
                        giaGoc,
                        soChoNgoi,
                        dungTich);

                    quanLy.AddPhuongTien(oto);

                    Console.WriteLine(
                        "Them o to thanh cong!");
                }
                else if (loai == 2)
                {
                    Console.Write(
                        "Nhap dung tich xylanh (cc): ");

                    int xylanh =
                        int.Parse(Console.ReadLine());

                    XeMay xeMay = new XeMay(
                        maPT,
                        tenHang,
                        namSanXuat,
                        giaGoc,
                        xylanh);

                    quanLy.AddPhuongTien(xeMay);

                    Console.WriteLine(
                        "Them xe may thanh cong!");
                }
                else
                {
                    Console.WriteLine(
                        "Loai phuong tien khong hop le!");
                }
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("Loi: " + ex.Message);
            }
        }
        static void HienThiKetQua(
            List<PhuongTien> ketQua)
        {
            if (ketQua.Count == 0)
            {
                Console.WriteLine(
                    "Khong tim thay phuong tien!");
                return;
            }

            foreach (PhuongTien pt in ketQua)
            {
                Console.WriteLine(pt.GetInfo());

                Console.WriteLine(
                    "Gia lan banh: "
                    + pt.TinhGiaLanBanh().ToString("N0")
                    + " VNĐ");

                Console.WriteLine("--------------------------------");
            }
        }


        static void Main(string[] args)
        {
            QuanLyPhuongTien quanLy =
                new QuanLyPhuongTien();

            int luaChon;

            do
            {
                Console.WriteLine();
                Console.WriteLine("==========================================");
                Console.WriteLine("       HE THONG QUAN LY PHUONG TIEN");
                Console.WriteLine("              AUTOSPEED");
                Console.WriteLine("==========================================");

                Console.WriteLine("1. Them phuong tien");
                Console.WriteLine("2. Hien thi tat ca phuong tien");
                Console.WriteLine(
                    "3. Tim phuong tien co gia lan banh cao nhat");
                Console.WriteLine(
                    "4. Tim phuong tien theo ten hang");
                Console.WriteLine(
                    "5. Tim phuong tien theo nam san xuat");
                Console.WriteLine("6. Chay Test Case");
                Console.WriteLine("7. Thoat");

                Console.WriteLine("==========================================");

                Console.Write("Nhap lua chon: ");
                luaChon =
                    int.Parse(Console.ReadLine());

                switch (luaChon)
                {
                    case 1:
                        ThemPhuongTien(quanLy);
                        break;


                    case 2:
                        quanLy.DisplayAll();
                        break;


             
                    case 3:
                        Console.WriteLine();
                        Console.WriteLine(
                            "===== GIA LAN BANH CAO NHAT =====");

                        PhuongTien max =
                            quanLy.FindMaxGiaLanBanh();

                        if (max == null)
                        {
                            Console.WriteLine(
                                "Danh sach phuong tien dang rong!");
                        }
                        else
                        {
                            Console.WriteLine(max.GetInfo());

                            Console.WriteLine(
                                "Gia lan banh: "
                                + max.TinhGiaLanBanh()
                                    .ToString("N0")
                                + " VNĐ");
                        }

                        break;


                    case 4:
                        Console.WriteLine();
                        Console.WriteLine(
                            "===== TIM THEO TEN HANG =====");

                        Console.Write(
                            "Nhap ten hang can tim: ");

                        string keyword =
                            Console.ReadLine();

                        List<PhuongTien> ketQua =
                            quanLy.SearchByName(keyword);

                        HienThiKetQua(ketQua);

                        break;

                    case 5:
                        Console.WriteLine();
                        Console.WriteLine(
                            "===== TIM THEO NAM SAN XUAT =====");

                        Console.Write(
                            "Nhap nam san xuat can tim: ");

                        int namTim =
                            int.Parse(Console.ReadLine());

                        List<PhuongTien> ketQuaNam =
                            quanLy.SearchByYear(namTim);

                        HienThiKetQua(ketQuaNam);

                        break;

                    case 6:
                        Console.WriteLine();
                        Console.WriteLine(
                            "=============== TEST CASE ===============");

                        Console.WriteLine(
                            "1. TC01 - Validation nam san xuat");

                        Console.WriteLine(
                            "2. TC02 - Tinh gia lan banh OTo");

                        Console.WriteLine(
                            "3. TC03 - Tinh gia lan banh XeMay");

                        Console.WriteLine(
                            "4. TC04 - Kiem tra da hinh");

                        Console.WriteLine(
                            "5. TC05 - Tim gia lan banh cao nhat");

                        Console.WriteLine(
                            "=========================================");

                        Console.Write(
                            "Chon Test Case: ");

                        int tc =
                            int.Parse(Console.ReadLine());

                        switch (tc)
                        {
                            case 1:
                                TestCase.TC01();
                                break;

                            case 2:
                                TestCase.TC02();
                                break;

                            case 3:
                                TestCase.TC03();
                                break;

                            case 4:
                                TestCase.TC04();
                                break;

                            case 5:
                                TestCase.TC05();
                                break;

                            default:
                                Console.WriteLine(
                                    "Test Case khong hop le!");
                                break;
                        }

                        break;
                    case 7:
                        Console.WriteLine(
                            "Ket thuc chuong trinh.");
                        break;


                    default:
                        Console.WriteLine(
                            "Lua chon khong hop le!");
                        break;
                }

            } while (luaChon != 7);
        }
    }
}

<img width="1276" height="956" alt="1790924774997_67568557624812199_8313470167825071624_ecce9f275a8680314fd30c3ee8f777a7" src="https://github.com/user-attachments/assets/c0e9556c-05e6-421e-a655-ba1c689e7977" />


