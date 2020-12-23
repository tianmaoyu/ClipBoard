```c#
public abstract class DatatablesQuery
    {
        /// <summary>
        /// datatables 默认的 start
        /// </summary>
        public int start { get; set; }

        /// <summary>
        /// datatables 默认的 length
        /// </summary>
        public int length { get; set; }

        public OrderByItem[] order { get; set; }

        /// <summary>
        /// datatables 默认的 draw
        /// </summary>
        public int draw { get; set; }


        /// <summary>
        /// 分页大小
        /// </summary>
        public int PageSize
        {
            get
            {
                return this.length;
            }
        }
        /// <summary>
        /// 当前页 的 index
        /// </summary>
        public int PageIndex
        {
            get
            {
                return start <= 0 ? 0 : (int)Math.Ceiling((decimal)start / this.PageSize);
            }
        }

        /// <summary>
        /// 排序
        /// </summary>
        public string Orderby
        {
            get
            {

                var orderType = this.order[0].dir;
                var orderList = this.GetOrderbyList();
                var columnName = orderList[this.order[0].column - 1];
                var result = string.Format("{0} {1}", columnName, (orderType == "desc" ? "desc" : ""));
                return result;
            }
        }



        /// <summary>
        /// 得到 order  的 字段 让子类去实现 只要子类 的公共属性字段
        /// </summary>
        /// <returns></returns>
        protected abstract List<string> GetOrderbyList();



    }


    /// <summary>
    ///  排序项
    /// </summary>
    public class OrderByItem
    {
        private int _column;
        private string _dir;

        public int column
        {
            get
            {
                return _column;

            }
            set
            {
                _column = value;
            }
        }

        public string dir
        {
            get
            {
                if (_dir == "desc")
                {
                    return "desc";
                }
                else
                {
                    return "asc";
                }
            }
            set
            {
                _dir = value;
            }
        }
    }
    
      public class ClientLogPageQuery : DatatablesQuery
    {

        public string ID { get; set; }

        public string UserIdentify { get; set; }
        /// <summary>
        /// ClientLogType 枚举
        /// </summary>
        public int? ClientLogType { get; set; }


        public string ClientName { get; set; }

        /// <summary>
        /// UserType 枚举
        /// </summary>
        public int? UserType { get; set; }

        public DateTime? CreateTimeStart { get; set; }

        public DateTime? CreateTimeEnd { get; set; }


        protected override List<string> GetOrderbyList()
        {
            var list = new List<string>();
            var type = this.GetType();
            //不需要父类的数据
            var propertyList = type.GetProperties(BindingFlags.Public | BindingFlags.DeclaredOnly | BindingFlags.Instance);
            foreach (var item in propertyList)
            {
                list.Add(item.Name);
            }
            return list;
        }
    }
    
    
      public class FramworkLogPageQuery : DatatablesQuery
    {

        public string ID { get; set; }

        public string UserIdentify { get; set; }

        public string IP { get; set; }

        public string Url { get; set; }

        public double? Elapsed { get; set; }

        public DateTime? CreateTimeStart { get; set; }

        public DateTime? CreateTimeEnd { get; set; }

        public int? LogType { get; set; }

        public string InputData { get; set; }

        protected override List<string> GetOrderbyList()
        {
            var list = new List<string>();
            var type = this.GetType();
            //不需要父类的数据
            var propertyList = type.GetProperties(BindingFlags.Public | BindingFlags.DeclaredOnly | BindingFlags.Instance);
            foreach (var item in propertyList)
            {
                list.Add(item.Name);
            }
            return list;
        }
    }
    ```
    
    
