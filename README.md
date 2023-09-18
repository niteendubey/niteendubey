using IBS.DataAccess;
using IBS.Interfaces;
using IBS.Interfaces.Reports;
using IBS.Interfaces.Vendor;
using IBS.Models;
using IBS.Repositories;
using Microsoft.AspNetCore.Mvc;
using System.Drawing;


namespace IBS.Controllers
{
    public class VendorWiseInspectionStatusController : Controller
    {

        #region Variables
        private readonly IVendor_Wise_Inspection_StatusRepository vendor_Wise_Inspection_StatusRepository;
        private readonly IWebHostEnvironment env;
        private readonly IConfiguration _config;
        #endregion

        public VendorWiseInspectionStatusController(IVendor_Wise_Inspection_StatusRepository _vendor_Wise_Inspection_StatusRepository, IWebHostEnvironment _environment, IConfiguration configuration)
        {
            vendor_Wise_Inspection_StatusRepository = _vendor_Wise_Inspection_StatusRepository;
            env = _environment;  
            _config = configuration;
        }

        public IActionResult Index()
        {
            return View();
        }
       
    }
}
 namespace IBS.Controllers
{
    internal class Vendor_Wise_Inspection_StatusModel
    {
        public object VendorWiseInspectionStatus { get; internal set; }
        public object AllVendors { get; internal set; }
        public object ForGivenPeriod { get; internal set; }
        public object ForTheMonth { get; internal set; }
        public object Month { get; internal set; }
        public object Year { get; internal set; }
        //public model.ForParticular ForParticularPeriod { get; internal set; }
        //public model.ForParticular ForParticularVendors { get; internal set; }
        public object?[]? Region { get; internal set; }
        //public model.ForParticular ForParticularPeriod { get; internal set; }
        //public model.ForParticular ForParticularVendors { get; internal set; }
        //public model.ForParticular ForParticularVendors { get; internal set; }
        //public model.ForParticular ForParticularPeriod { get; internal set; }
    }
}
using IBS.Controllers;
using IBS.Models;

namespace IBS.Interfaces.Vendor
{
    public interface IVendor_Wise_Inspection_StatusRepository
    {
        DTResult<PendingJICasesReportModel> Get_Pending_JI_Cases(DTParameters dtParameters, string iecd);
        DTResult<IEDairyModel> Get_IE_Dairy(DTParameters dtParameters, UserSessionModel userModel);
    }
}
@{
    ViewData["Title"] = " VENDOR WISE INSPECTION STATUS FORM";
}

<div>
    <div class="list-inner">
        <div class="tast-list">
            <h2>VENDOR WISE INSPECTION STATUS FORM</h2>
        </div>
        <div class="list-btn">
            <a asp-area="" asp-controller="Vendor_Wise_Inspection_Status" asp-action="Vendor_Wise_Inspection_StatuseManage" class="formBtn"><span class="fa fa-plus"></span> Vendor Wise Inspection Status</a>
        </div>
    </div>
    <section class="table-section">
        <div class="task-listinput">
            <div class="dash-table">
                <table id="dtList" class="table-responsive">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Region</th>
                            <th>Vendor Wise Inspection Status</th>
                            <th>AllVendors</th>
                            <th>ForParticularVendors</th>
                            <th>ForTheMonth</th>
                            <th>ForGivenPeriod</th>
                            <th>Month</th>
                            <th>Year</th>
                           
                            
                           
                        </tr>
                    </thead>
                    <tbody>
                    </tbody>
                </table>
            </div>
        </div>
    </section>
</div>

@section scripts{

    <partial name="_DataTablesScriptsPartial" />
    <partial name="_ValidationScriptsPartial" />

    <script type="text/javascript">

        $(function () {
            InitializeDatatable();
        });

        function InitializeDatatable() {

            $("#dtList").DataTable({
                stateSave: false,// Design Assets
                autoWidth: true,
                scrollX: true,
                scrollCollapse: true,
                processing: true, // ServerSide Setups
                serverSide: true,
                destroy: true,
                paging: true,// Paging Setups
                searching: true,// Searching Setups
                ajax: {// Ajax Filter
                    url: "@Url.Action("LoadTable")",
                    type: "POST",
                    contentType: "application/json; charset=utf-8",
                    dataType: "json",
                    data: function (d) {
                        var AdditionalValues = {
                        };
                        d.AdditionalValues = AdditionalValues;
                        return JSON.stringify(d);
                    }
                },

                columns: [// Columns Setups
                    {
                        data: '', orderable: false, width: '5%',
                        render: function (data, type, row, meta) {
                            return meta.row + meta.settings._iDisplayStart + 1;
                        }
                    },

                    { data: "Region" },
                    { data: "VendorWiseInspectionStatus" },
                    { data: "AllVendors" },
                    { data: "ForParticularVendors" },
                    { data: "ForTheMonth" },
                    { data: "ForGivenPeriod" },
                    { data: "Month" },
                    { data: "Year" },
                   
                   
                    {
                        data: null, orderable: false,
                        render: function (data, type, row) {
                            var Id = data.Region;
                            var editUrl = '@Url.Action("Vendor_Wise_Inspection_StatusManage", "Vendor_Wise_Inspection_Status")?region=' + Id;
                            var html = '<div align=\"center\" class=\"reportIcon\"> <a href=\"' + editUrl + '\" class=\"edit\"><i class=\"fa fa-pencil\" title="Edit"></i></a>';
                            //html += '<a onclick="MessageDelete(\'' + Id + '\'); return false;" href="javascript:void(\'0\');" id=\"' + Id + '\" class=\"delete\"><i class=\"fa fa-trash\" title="Delete" style="margin:10px;"></i>';
                            html += '</div>';
                            return html;
                        }
                    },
                ],
                "order": [[0, "asc"]]
            });
        }

                //function MessageDelete(MessageID) {
                //    var url = '@Url.Action("Delete", "GeneralMessages")?MessageID=' + MessageID;
                //    $("#btn-delete-yes").attr("href", url);
                //    $("#modal-delete-conf").modal("show");
                //}

    </script>
}
@model IBS.Models.Vendor_Wise_Inspection_StatusModel

@{
    ViewData["Title"] = "Manage VENDOR WISE INSPECTION STATUS FORM";
}

<div class="list-inner">
    <div class="tast-list">
        <h2>@ViewData["Title"] </h2>
    </div>
    <input type="hidden" asp-for="Region" />
</div>
<div class="accordion-body">
    <div class="row my-0">
       <div class="col-md-4 mb-3">
            <label for="PropertyId">Region</label>
            @Html.DropDownListFor(model => model.Region, Common.GetRegionType(), new { })
            <span asp-validation-for="Region" class="text-danger"></span>
        </div>
        <div class="col-md-4 mb-3">
            <div class="custom-readio">
                <label for="Reference">Vendor Wise Inspection Status (Y/N)</label>
                <div class="company-checkbox">
                    <div class="remember">
                        <div class="remecheckbox">
                            <input type="radio" asp-for="VendorWiseInspectionStatus" value="Y" id="VendorWiseInspectionStatus_Y" />
                            <label for="VendorWiseInspectionStatus_Y">Yes</label>
                            &nbsp;&nbsp;
                            <input type="radio" asp-for="VendorWiseInspectionStatus" value="N" id="VendorWiseInspectionStatus_N" />
                            <label for="VendorWiseInspectionStatus_N">No</label>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-md-4 mb-3">
            <label for="Reference">AllVendors</label>
            <input type="text" class="input" asp-for="AllVendors" maxlength="30" style="text-transform: uppercase;">
            <span asp-validation-for="AllVendors" class="text-danger"></span>
        </div>
    </div>
    <div class="savenext-btn">
        <a asp-controller="Vendor_Wise_Inspection_Status" asp-action="Vendor_Wise_Inspection_StatusMaster" class="reset-btn">Cancel</a>
        <button type="button" class="save-btn active" onclick="Save();">Save</button>
    </div>
</div>
@section scripts{
    <partial name="_DataTablesScriptsPartial" />
    <partial name="_ValidationScriptsPartial" />
    <script>
        function Save() {
            if ($("#frmVendorWiseInspectionStatusDetails").valid()) {
                $("#frmVendorWiseInspectionStatusDetails").submit();
            }
        }
        completed = function (response) {
            var res = response.responseJSON;
            ShowHideMsgNew(res.status, res.responseText);
            //window.location.href = "/Allow_Old_Bill_Date/Allow_Old_Bill_DateMaster";
            window.location.href = '@Url.Action("Vendor_Wise_Inspection_StatusMaster", "Vendor_Wise_Inspection_Status")';
        };
    </script>
}
