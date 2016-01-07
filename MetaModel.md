
```
<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="1.0" xmlns:edmx="http://schemas.microsoft.com/ado/2007/06/edmx" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata">
    <edmx:DataServices m:DataServiceVersion="1.0">
        <Schema xmlns="http://schemas.microsoft.com/ado/2006/04/edm" Namespace="com.microsoft.schemas.ado._2008._09.edm">
            <ComplexType Name="PropertyRef">
                <Property Name="Name" Type="Edm.String" Nullable="false"></Property>
            </ComplexType>
            <ComplexType Name="EntityKey">
                <Property Name="Keys" Type="com.microsoft.schemas.ado._2008._09.edm.PropertyRef" Nullable="false" CollectionKind="List"></Property>
            </ComplexType>
            <ComplexType Name="Documentation">
                <Property Name="Summary" Type="Edm.String" Nullable="true"></Property>
                <Property Name="LongDescription" Type="Edm.String" Nullable="true"></Property>
            </ComplexType>
            <EntityType Name="Schema">
                <Key>
                    <PropertyRef Name="Namespace"></PropertyRef>
                </Key>
                <Property Name="Namespace" Type="Edm.String" Nullable="false"></Property>
                <Property Name="Alias" Type="Edm.String" Nullable="true"></Property>
                <NavigationProperty Name="EntityTypes" Relationship="com.microsoft.schemas.ado._2008._09.edm.EntityTypes" FromRole="Schema" ToRole="StructuralType"></NavigationProperty>
                <NavigationProperty Name="ComplexTypes" Relationship="com.microsoft.schemas.ado._2008._09.edm.ComplexTypes" FromRole="Schema" ToRole="ComplexType"></NavigationProperty>
            </EntityType>
            <EntityType Name="StructuralType">
                <Key>
                    <PropertyRef Name="Namespace"></PropertyRef>
                    <PropertyRef Name="Name"></PropertyRef>
                </Key>
                <Property Name="Namespace" Type="Edm.String" Nullable="false"></Property>
                <Property Name="Name" Type="Edm.String" Nullable="false"></Property>
                <Property Name="BaseType" Type="Edm.String" Nullable="true"></Property>
                <Property Name="Abstract" Type="Edm.Boolean" Nullable="true"></Property>
                <NavigationProperty Name="Properties" Relationship="com.microsoft.schemas.ado._2008._09.edm.Properties" FromRole="StructuralType" ToRole="Property"></NavigationProperty>
                <NavigationProperty Name="SubTypes" Relationship="com.microsoft.schemas.ado._2008._09.edm.SubTypes" FromRole="StructuralType" ToRole="StructuralType1"></NavigationProperty>
                <NavigationProperty Name="SuperType" Relationship="com.microsoft.schemas.ado._2008._09.edm.SuperType" FromRole="StructuralType" ToRole="StructuralType1"></NavigationProperty>
            </EntityType>
            <EntityType Name="ComplexType" BaseType="com.microsoft.schemas.ado._2008._09.edm.StructuralType"></EntityType>
            <EntityType Name="EntityType" BaseType="com.microsoft.schemas.ado._2008._09.edm.StructuralType">
                <Property Name="Key" Type="com.microsoft.schemas.ado._2008._09.edm.EntityKey" Nullable="true"></Property>
            </EntityType>
            <EntityType Name="Property">
                <Key>
                    <PropertyRef Name="Namespace"></PropertyRef>
                    <PropertyRef Name="EntityTypeName"></PropertyRef>
                    <PropertyRef Name="Name"></PropertyRef>
                </Key>
                <Property Name="Namespace" Type="Edm.String" Nullable="false"></Property>
                <Property Name="EntityTypeName" Type="Edm.String" Nullable="false"></Property>
                <Property Name="Name" Type="Edm.String" Nullable="false"></Property>
                <Property Name="Type" Type="Edm.String" Nullable="false"></Property>
                <Property Name="Nullable" Type="Edm.Boolean" Nullable="true"></Property>
                <Property Name="DefaultValue" Type="Edm.String" Nullable="true"></Property>
                <Property Name="MaxLength" Type="Edm.Int32" Nullable="true"></Property>
                <Property Name="FixedLength" Type="Edm.Boolean" Nullable="true"></Property>
                <Property Name="Precision" Type="Edm.Int16" Nullable="true"></Property>
                <Property Name="Scale" Type="Edm.Int16" Nullable="true"></Property>
                <Property Name="Unicode" Type="Edm.Boolean" Nullable="true"></Property>
                <Property Name="Collation" Type="Edm.String" Nullable="true"></Property>
                <Property Name="ConcurrencyMode" Type="Edm.String" Nullable="true"></Property>
            </EntityType>
            <Association Name="EntityTypes">
                <End Role="Schema" Type="com.microsoft.schemas.ado._2008._09.edm.Schema" Multiplicity="1"></End>
                <End Role="StructuralType" Type="com.microsoft.schemas.ado._2008._09.edm.StructuralType" Multiplicity="*"></End>
            </Association>
            <Association Name="ComplexTypes">
                <End Role="Schema" Type="com.microsoft.schemas.ado._2008._09.edm.Schema" Multiplicity="1"></End>
                <End Role="ComplexType" Type="com.microsoft.schemas.ado._2008._09.edm.ComplexType" Multiplicity="*"></End>
            </Association>
            <Association Name="Properties">
                <End Role="StructuralType" Type="com.microsoft.schemas.ado._2008._09.edm.StructuralType" Multiplicity="1"></End>
                <End Role="Property" Type="com.microsoft.schemas.ado._2008._09.edm.Property" Multiplicity="*"></End>
            </Association>
            <Association Name="SubTypes">
                <End Role="StructuralType" Type="com.microsoft.schemas.ado._2008._09.edm.StructuralType" Multiplicity="1"></End>
                <End Role="StructuralType1" Type="com.microsoft.schemas.ado._2008._09.edm.StructuralType" Multiplicity="*"></End>
            </Association>
            <Association Name="SuperType">
                <End Role="StructuralType" Type="com.microsoft.schemas.ado._2008._09.edm.StructuralType" Multiplicity="1"></End>
                <End Role="StructuralType1" Type="com.microsoft.schemas.ado._2008._09.edm.StructuralType" Multiplicity="0..1"></End>
            </Association>
            <EntityContainer Name="EdmContainer" m:IsDefaultEntityContainer="true">
                <EntitySet Name="Schemas" EntityType="com.microsoft.schemas.ado._2008._09.edm.Schema"></EntitySet>
                <EntitySet Name="ComplexTypes" EntityType="com.microsoft.schemas.ado._2008._09.edm.ComplexType"></EntitySet>
                <EntitySet Name="RootComplexTypes" EntityType="com.microsoft.schemas.ado._2008._09.edm.ComplexType"></EntitySet>
                <EntitySet Name="EntityTypes" EntityType="com.microsoft.schemas.ado._2008._09.edm.EntityType"></EntitySet>
                <EntitySet Name="RootEntityTypes" EntityType="com.microsoft.schemas.ado._2008._09.edm.EntityType"></EntitySet>
                <EntitySet Name="Properties" EntityType="com.microsoft.schemas.ado._2008._09.edm.Property"></EntitySet>
                <AssociationSet Name="EntityTypes" Association="com.microsoft.schemas.ado._2008._09.edm.EntityTypes">
                    <End Role="Schema" EntitySet="Schemas"></End>
                    <End Role="StructuralType" EntitySet="EntityTypes"></End>
                </AssociationSet>
                <AssociationSet Name="ComplexTypes" Association="com.microsoft.schemas.ado._2008._09.edm.ComplexTypes">
                    <End Role="Schema" EntitySet="Schemas"></End>
                    <End Role="ComplexType" EntitySet="ComplexTypes"></End>
                </AssociationSet>
                <AssociationSet Name="Properties" Association="com.microsoft.schemas.ado._2008._09.edm.Properties">
                    <End Role="StructuralType" EntitySet="EntityTypes"></End>
                    <End Role="Property" EntitySet="Properties"></End>
                </AssociationSet>
                <AssociationSet Name="SubTypes" Association="com.microsoft.schemas.ado._2008._09.edm.SubTypes">
                    <End Role="StructuralType" EntitySet="EntityTypes"></End>
                    <End Role="StructuralType1" EntitySet="EntityTypes"></End>
                </AssociationSet>
                <AssociationSet Name="SuperType" Association="com.microsoft.schemas.ado._2008._09.edm.SuperType">
                    <End Role="StructuralType" EntitySet="EntityTypes"></End>
                    <End Role="StructuralType1" EntitySet="EntityTypes"></End>
                </AssociationSet>
            </EntityContainer>
        </Schema>
    </edmx:DataServices>
</edmx:Edmx>
```