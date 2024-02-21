// validate contract item onSelect
// 获取当前选择的记录
Set(
    currContractitem,
    ContractItemGallary.Selected
);
Set(
    currentContract,
    DistContractGallary.Selected
);
Set(
    currCity,
    Split(
        currContractitem.RegionValue,
        ","
    )
);
//Notify( currContractitem.RegionValue);
// 检查是否存在与当前记录在指定字段上有重叠值的其他记录
ClearCollect(
    overlappingRecordsA,
    Filter(
        DistContractDetail,
        ID <> currContractitem.ID && Region.Value = currContractitem.Region.Value && DistID <> currContractitem.DistID && BUStr = currentContract.BU.Value && BIN.Value = currContractitem.BIN.Value && SBS = currContractitem.SBS
    )
);
Set(
    currLine,
    Split(
        currContractitem.ProductLine,
        ","
    )
);
If(
    currContractitem.LineScope.Value = "所有产品线",
    UpdateContext(
        {
            numProduct: CountRows(
                Filter(
                    Products,
                    SBS = currContractitem.SBS,
                    BIN = binEN
                )
            )
        }
    ),
    UpdateContext(
        {
            numProduct: CountRows(
                Filter(
                    Products,
                    SBS = currContractitem.SBS,
                    BIN = binEN,
                    ProductLine in currContractitem.ProductLine
                )
            )
        }
    )
);
Set(
    currProduct,
    Split(
        currContractitem.ProductValue,
        ","
    )
);
Set(
    currHosptial,
    Split(
        currContractitem.Hosptial,
        ","
    )
);
Set(
    currDept,
    Split(
        currContractitem.Department,
        ","
    )
);
//ClearCollect(currProd, currProduct.Value);
UpdateContext({currLine: currLine});
Notify(
    Concatenate(
        "正在对比 : ",
        CountRows(overlappingRecordsA),
        " 个合同行"
    ),
    NotificationType.Warning,
    4000
);
//Step 1 find confict city
Clear(overlappingRecords);
Clear(overlappingRecords2);
ClearCollect(
    overlappingRecords,
    AddColumns(
        overlappingRecordsA,
        "currRegionScope",
        currContractitem.RegionScope.Value,
        "currLineScope",
        currContractitem.LineScope.Value,
        "currProductScope",
        currContractitem.ProductScope.Value,
        "currLine",
        currContractitem.ProductLine,
        "currProduct",
        currContractitem.ProductValue,
        "currHospitalScope",
        currContractitem.HospitalScope.Value,
        "currDeptScope",
        currContractitem.DepartmentScope.Value,
        "currCustScope",
        currContractitem.CustomerScope,
        "RegionConf",
        false,
        "LineConf",
        false,
        "ProductConf",
        false,
        "prodV",
        "",// for validate prodv
        "HospitalConf",
        false,
        "DeptConf",
        false,
        "CustomerConf",
        false,
        "numProduct",
        numProduct,
        "numCurrProd",
        currProduct,
        "numConfit",
        0
    );
    
);
Collect(
    overlappingRecords2,//copy to checking result table
    overlappingRecords
);
ForAll(
    overlappingRecords As record,
    If(
        record.currRegionScope = "所有区域" || record.RegionScope.Value = "所有区域",
        Patch(
            overlappingRecords2,
            LookUp(
                overlappingRecords2,
                ID = record.ID
            ),
            {RegionConf: true}
        ),
        ForAll(
            currCity As city,
            If(
                !IsBlank(
                    Find(
                        city.Value,
                        record.RegionValue
                    )
                ),
                Patch(
                    overlappingRecords2,
                    LookUp(
                        overlappingRecords2,
                        ID = record.ID
                    ),
                    {RegionConf: true}
                )
            )
        )
    )
);
// step 2 validate productLine confict
ForAll(
    overlappingRecords As record,
    If(
        record.currLineScope = "所有产品线" || record.LineScope.Value = "所有产品线",
        Patch(
            overlappingRecords2,
            LookUp(
                overlappingRecords2,
                ID = record.ID
            ),
            {LineConf: true}
        ),
        If(
            // 如果包含，
           CountRows(
                        Distinct(
                            Split(
                                Concatenate(
                                    currContractitem.ProductLine,
                                    ",",
                                    record.ProductLine
                                ),
                                ","
                            ),
                            Value
                        )
                    ) < CountRows(
                        Split(
                            Concatenate(
                                currContractitem.ProductLine,
                                ",",
                                record.ProductLine
                            ),
                            ","
                        )
                    ), 
                    Patch(
                    overlappingRecords2,
                    LookUp(
                        overlappingRecords2,
                        ID = record.ID
                    ),
                    {LineConf: true}
                ),
                false
        )
    )
);
ForAll(
    overlappingRecords As record,
    If(
        record.currProductScope = "所有产品" || record.ProductScope.Value = "所有产品",
        Patch(
            overlappingRecords2,
            LookUp(
                overlappingRecords2,
                ID = record.ID
            ),
            {ProductConf: true}
        ),
        // current product op = include 
        If(
            record.currProductScope = "包含以下产品",
            If(
                // other = include,  有重复
                record.ProductScope.Value = "包含以下产品",
                If(
                    CountRows(
                        Distinct(
                            Split(
                                Concatenate(
                                    currContractitem.ProductValue,
                                    ",",
                                    record.ProductValue
                                ),
                                ","
                            ),
                            Value
                        )
                    ) < CountRows(
                        Split(
                            Concatenate(
                                currContractitem.ProductValue,
                                ",",
                                record.ProductValue
                            ),
                            ","
                        )
                    ),
                    Patch(
                        overlappingRecords2,
                        LookUp(
                            overlappingRecords2,
                            ID = record.ID
                        ),
                        {ProductConf: true}
                    ),
                    false
                ),
                // other = exclude
                If(
                    CountRows(
                        Distinct(
                            Split(
                                Concatenate(
                                    currContractitem.ProductValue,
                                    ",",
                                    record.ProductValue
                                ),
                                ","
                            ),
                            Value
                        )
                    ) > CountRows(
                        Split(
                            record.ProductValue,
                            ","
                        )
                    ),
                    Patch(
                        overlappingRecords2,
                        LookUp(
                            overlappingRecords2,
                            ID = record.ID
                        ),
                        {ProductConf: true}
                    ),
                    false
                )
            ),
            // current product op = exclude "除以下产品外所有产品" 
// record op = exclude "包含以下产品" 
            If(
                record.ProductScope.Value = "包含以下产品",
                If(
                    CountRows(
                        Distinct(
                            Split(
                                Concatenate(
                                    currContractitem.ProductValue,
                                    ",",
                                    record.ProductValue
                                ),
                                ","
                            ),
                            Value
                        )
                    ) > CountRows(
                        Split(
                            currContractitem.ProductValue,
                            ","
                        )
                    ),
                    Patch(
                        overlappingRecords2,
                        LookUp(
                            overlappingRecords2,
                            ID = record.ID
                        ),
                        {ProductConf: true}
                    ),
                    false
                ),
            // record op = exclude "除以下产品外所有产品" 
                If(
                    CountRows(
                        Distinct(
                            Split(
                                Concatenate(
                                    currContractitem.ProductValue,
                                    ",",
                                    record.ProductValue
                                ),
                                ","
                            ),
                            Value
                        )
                    ) < numProduct,
                    Patch(
                        overlappingRecords2,
                        LookUp(
                            overlappingRecords2,
                            ID = record.ID
                        ),
                        {ProductConf: true}
                    ),
                    false
                )
            )
        )
    )
);
// step 3 validate customer or hospital conflict
If(
    DistContractGallary.Selected.AuthType.Value = "临床",
    ForAll(
        overlappingRecords As record,
        If(
            record.HospitalScope.Value = "所有客户" || record.currHospitalScope = "所有客户",
            Patch(
                overlappingRecords2,
                LookUp(
                    overlappingRecords2,
                    ID = record.ID
                ),
                {HospitalConf: true}
            ),
            ForAll(
                currHosptial As hosp,
                If(
                    !IsBlank(
                        Find(
                            hosp.Value,
                            record.Hosptial
                        )
                    ),
                    Patch(
                        overlappingRecords2,
                        LookUp(
                            overlappingRecords2,
                            ID = record.ID
                        ),
                        {HospitalConf: true}
                    )
                )
            )
        )
    ),
    false
);
// step 4 validate customer or dept conflict
If(
    DistContractGallary.Selected.AuthType.Value = "临床",
    ForAll(
        overlappingRecords As record,
        If(
            record.DepartmentScope.Value = "所有科室" || record.currDeptScope = "所有科室",
            Patch(
                overlappingRecords2,
                LookUp(
                    overlappingRecords2,
                    ID = record.ID
                ),
                {DeptConf: true}
            ),
            ForAll(
                currDept As dept,
                If(
                    !IsBlank(
                        Find(
                            dept.Value,
                            record.Department
                        )
                    ),
                    Patch(
                        overlappingRecords2,
                        LookUp(
                            overlappingRecords2,
                            ID = record.ID
                        ),
                        {DeptConf: true}
                    )
                )
            )
        )
    ),
    false
);
// final: if city + productline + customer conflict, mark conflict
If(
    CountRows(
        Filter(
            overlappingRecords2,
            RegionConf && LineConf && ProductConf && HospitalConf && DeptConf
        )
    ) > 0,
    Notify(
        Concatenate(
            "当前合同行与: ",
            CountRows(
                Filter(
                    overlappingRecords2,
                    RegionConf && LineConf && ProductConf && HospitalConf && DeptConf
                )
            ),
            " 个合同行有冲突"
        ),
        NotificationType.Warning,
        4000
    ),
    Notify(
        "当前合同行没有与其它合同冲突",
        NotificationType.Success,
        3000
    )
);
If(
    CountRows(
        Filter(
            overlappingRecords2,
            RegionConf && LineConf && ProductConf && HospitalConf && DeptConf
        )
    ) > 0,
    Navigate(ContractItemConflictS1),
    false
);
//Navigate(ContractItemConflictS1);
