<?xml version="1.0" encoding="UTF-8"?>

<!-- must be put in json-file on level null -->
<!-- abstract> -->
<!-- name>IABO Configuration File</name -->
<!-- description>MP15 PS Siemens </description -->
<!-- techReleaseNotes>Draft for IABO configuration file</techReleaseNotes -->
<!-- material>1234567890</material -->
<!-- uid>12345</uid -->
<!-- version major="1" minor="0" revision="0"/ -->
<!-- /abstract -->
<IABO_Config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <!-- xsi:noNamespaceSchemaLocation="IABOcfg.xsd" -->  
  <UIM-Domain_IABO_common> 
    <description>MP15 Ps Bosch PS OS</description>  
    <version major="1" minor="0" revision="0"/>  
    <LEDModes> 
      <LEDModesFix> 
        <!--Predefined led mode in IABO, do NOT change them-->  
        <LedMode name="Off" id="0"/>  
        <LedMode name="On" id="1"/>  
        <LedMode name="FlashRate_1" id="2"/>  
        <LedMode name="FlashRate_2" id="3"/>  
        <LedMode name="FlashRate_3" id="4"/>  
        <LedMode name="FlashRate_4" id="5"/>  
        <LedMode name="FlashRate_5" id="6"/> 
      </LEDModesFix>  
      <LEDModesBin> 
        <LedModeDefinition> 
          <!-- only one LedMode and LedModeConfig per LEDModeDefinition /-->  
          <LedMode name="FlashRate_6" id="7"/>  
          <LedModeConfig repetitions="255" sync="1" brightnessInitial="50"> 
            <LedModeConfigEntries> 
              <!-- number of entries min=1 max=9/-->  
              <LedModeConfigEntry entryId="0" TransitionTime="500" BrightnessEnd="50"/>  
              <LedModeConfigEntry entryId="1" TransitionTime="0" BrightnessEnd="100"/>  
              <LedModeConfigEntry entryId="2" TransitionTime="500" BrightnessEnd="100"/> 
            </LedModeConfigEntries> 
          </LedModeConfig> 
        </LedModeDefinition> 
      </LEDModesBin>  
      <LEDModesCustom> 
        <LedModeDefinition> 
          <!--Customer defined led mode/-->  
          <!-- only one LedMode and LedModeConfig per LEDModeDefinition /-->  
          <LedMode name="Dimming" id="9"/>  
          <LedModeConfig repetitions="0" sync="1" brightnessInitial="50"> 
            <LedModeConfigEntries> 
              <!-- number of entries min=1 max=81/-->  
              <LedModeConfigEntry entryId="0" TransitionTime="0" BrightnessEnd="50"/> 
            </LedModeConfigEntries> 
          </LedModeConfig> 
        </LedModeDefinition> 
      </LEDModesCustom> 
    </LEDModes>  
    <PhysicalAnimations> 
      <!-- no animation -->  
      <Unit name="Off" hwId="0x00"/>  
      <!-- SE -->  
      <Unit name="StartUpAttraction_1" hwId="0x02"/>  
      <!-- for not use now
            <Unit name="FunctionPhase_1" hwId="0x02"/>
            <Unit name="FirmwareUpdate_1" hwId="0x05"/>
            -->  
      <!-- BO -->  
      <Unit name="StartUpAttraction_2" hwId="0x06"/>  
      <!-- for not use now
            <Unit name="FunctionPhase_2" hwId="0x07"/>
            <Unit name="FirmwareUpdate_2" hwId="0x0A"/>
            -->  
      <!-- GA -->  
      <!--Unit name="StartUpAttraction_3"  hwId="0x0B"/ -->  
      <!--Unit name="FunctionPhase_3"  hwId="0x0C"/ -->  
      <!--Unit name="FirmwareUpdate_3"  hwId="0x0F"/ --> 
    </PhysicalAnimations>  
    <ButtonModes> 
      <ButtonMode name="ShortPress1" id="0" eventType="0x00" duration="100"/>  
      <ButtonMode name="LongPress1" id="1" eventType="0x01" duration="500"/>  
      <!--ButtonMode name="ShortPress2" id="2" eventType="0x00" duration="200" repetitions="10" RepeatDuration=="1000"/-->  
      <!--ButtonMode name="LongPress2" id="3" eventType="0x01" duration="500" repetitions="10" RepeatDuration=="1000"/--> 
    </ButtonModes> 
  </UIM-Domain_IABO_common>  
  <UIM-Domain_IABO_PS> 
    <description>MP15 Ps Bosch PS</description>  
    <version major="1" minor="0" revision="0"/>  
    <GeneralData operatingModulename="LC_UIM_MP15_01_04" nodeDBus2="0x22"/>  
    <InputUnits> 
      <Buttons> 
        <TouchButtons> 
          <Unit name="PS_Btn_1" hwId=""/>
          <Unit name="PS_Btn_2" hwId=""/>
          <Unit name="PS_Btn_3" hwId=""/>
        </TouchButtons> 
      </Buttons>  
      <Sliders> 
        <Slider sliderName="Slider_000"> 
          <Unit name="PS_Btn_1"/>  
          <Unit name="PS_Btn_2"/>  
          <Unit name="PS_Btn_3"/>  
          <Unit name="PS_Btn_4"/>  
          <Unit name="PS_Btn_5"/>  
          <Unit name="PS_Btn_6"/>  
          <Unit name="PS_Btn_7"/>  
          <Unit name="PS_Btn_8"/> 
        </Slider> 
      </Sliders> 
    </InputUnits>  
    <OutputUnits> 
      <LEDs> 
        <PhysicalLEDs> 
          <Unit name="PS_LED_1" hwId=""/>
          <Unit name="PS_LED_2" hwId=""/>
          <Unit name="PS_LED_3" hwId=""/>
          <Unit name="PS_LED_4" hwId=""/>
          <Unit name="PS_LED_5" hwId=""/>
          <Unit name="PS_LED_6" hwId=""/>
          <Unit name="PS_LED_7" hwId=""/>
          <Unit name="PS_LED_8" hwId=""/>
          <Unit name="PS_LED_9" hwId=""/>
          <Unit name="PS_LED_10" hwId=""/>
          <Unit name="PS_LED_11" hwId=""/>
          <Unit name="PS_LED_12" hwId=""/>
          <Unit name="PS_LED_13" hwId=""/>
          <Unit name="PS_LED_14" hwId=""/>
          <Unit name="PS_LED_15" hwId=""/>
          <Unit name="PS_LED_16" hwId=""/>
          <Unit name="PS_LED_17" hwId=""/>
          <Unit name="PS_LED_18" hwId=""/>
          <Unit name="PS_LED_19" hwId=""/>
          <Unit name="PS_LED_20" hwId=""/>
          <Unit name="PS_LED_21" hwId=""/>
          <Unit name="PS_LED_22" hwId=""/>
          <Unit name="PS_LED_23" hwId=""/>
          <Unit name="PS_LED_24" hwId=""/>
          <Unit name="PS_LED_25" hwId=""/>
          <Unit name="PS_LED_26" hwId=""/>
          <Unit name="PS_LED_27" hwId=""/>
          <Unit name="PS_LED_28" hwId=""/>
          <Unit name="PS_LED_29" hwId=""/>
          <Unit name="PS_LED_30" hwId=""/>
          <Unit name="PS_LED_31" hwId=""/>
          <Unit name="PS_LED_32" hwId=""/>
          <Unit name="PS_LED_33" hwId=""/>
          <Unit name="PS_LED_34" hwId=""/>
          <Unit name="PS_LED_35" hwId=""/>
          <Unit name="PS_LED_36" hwId=""/>
          <Unit name="PS_LED_37" hwId=""/>
          <Unit name="PS_LED_38" hwId=""/>
          <Unit name="PS_LED_39" hwId=""/>
          <Unit name="PS_LED_40" hwId=""/>
          <Unit name="PS_LED_41" hwId=""/>
          <Unit name="PS_LED_42" hwId=""/>
          <Unit name="PS_LED_43" hwId=""/>
          <Unit name="PS_LED_44" hwId=""/>
          <Unit name="PS_LED_45" hwId=""/>
          <Unit name="PS_LED_46" hwId=""/>
          <Unit name="PS_LED_47" hwId=""/>
          <Unit name="PS_LED_48" hwId=""/>
          <Unit name="PS_LED_49" hwId=""/>
          <Unit name="PS_LED_50" hwId=""/>
          <Unit name="PS_LED_51" hwId=""/>
          <Unit name="PS_LED_52" hwId=""/>
          <Unit name="PS_LED_53" hwId=""/>
          <Unit name="PS_LED_54" hwId=""/>
          <Unit name="PS_LED_55" hwId=""/>
          <Unit name="PS_LED_56" hwId=""/>
          <Unit name="PS_LED_57" hwId=""/>
          <Unit name="PS_LED_58" hwId=""/>
          <Unit name="PS_LED_59" hwId=""/>
          <Unit name="PS_LED_60" hwId=""/>
          <Unit name="PS_LED_61" hwId=""/>
        </PhysicalLEDs>  
        <VirtualSegments> 
          <VirtualSegment name="SegmentGroup1">
            <PhysicalUnit name="PS_LED_10"/>
            <PhysicalUnit name="PS_LED_4"/>
            <PhysicalUnit name="PS_LED_5"/>
            <PhysicalUnit name="PS_LED_6"/>
            <PhysicalUnit name="PS_LED_7"/>
            <PhysicalUnit name="PS_LED_8"/>
            <PhysicalUnit name="PS_LED_9"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup2">
            <PhysicalUnit name="PS_LED_13"/>
            <PhysicalUnit name="PS_LED_14"/>
            <PhysicalUnit name="PS_LED_15"/>
            <PhysicalUnit name="PS_LED_16"/>
            <PhysicalUnit name="PS_LED_17"/>
            <PhysicalUnit name="PS_LED_18"/>
            <PhysicalUnit name="PS_LED_19"/>
          </VirtualSegment>
          <VirtualSegment name="SGDOT">
            <PhysicalUnit name="PS_LED_11"/>
            <PhysicalUnit name="PS_LED_12"/>
          </VirtualSegment>
          <VirtualSegmentString name="SGSTRING">
            <VirtualUnit name="SGDOT"/>
            <VirtualUnit name="SegmentGroup1"/>
            <VirtualUnit name="SegmentGroup2"/>
          </VirtualSegmentString>
          <VirtualSegmentString name="SegmentGroup5">
            <PhysicalUnit name="PS_LED_20"/>
            <PhysicalUnit name="PS_LED_21"/>
            <PhysicalUnit name="PS_LED_22"/>
            <PhysicalUnit name="PS_LED_23"/>
            <PhysicalUnit name="PS_LED_24"/>
            <PhysicalUnit name="PS_LED_25"/>
            <PhysicalUnit name="PS_LED_26"/>
          </VirtualSegmentString>
          <VirtualSegmentString name="SegmentGroup6">
            <PhysicalUnit name="PS_LED_27"/>
            <PhysicalUnit name="PS_LED_28"/>
            <PhysicalUnit name="PS_LED_29"/>
            <PhysicalUnit name="PS_LED_30"/>
            <PhysicalUnit name="PS_LED_31"/>
            <PhysicalUnit name="PS_LED_32"/>
            <PhysicalUnit name="PS_LED_33"/>
          </VirtualSegmentString>
          <VirtualSegmentString name="SegmentGroup7">
            <PhysicalUnit name="PS_LED_34"/>
            <PhysicalUnit name="PS_LED_35"/>
            <PhysicalUnit name="PS_LED_36"/>
            <PhysicalUnit name="PS_LED_37"/>
            <PhysicalUnit name="PS_LED_38"/>
            <PhysicalUnit name="PS_LED_39"/>
            <PhysicalUnit name="PS_LED_40"/>
          </VirtualSegmentString>
          <VirtualSegmentString name="SegmentGroup8">
            <PhysicalUnit name="PS_LED_41"/>
            <PhysicalUnit name="PS_LED_42"/>
            <PhysicalUnit name="PS_LED_43"/>
            <PhysicalUnit name="PS_LED_44"/>
            <PhysicalUnit name="PS_LED_45"/>
            <PhysicalUnit name="PS_LED_46"/>
            <PhysicalUnit name="PS_LED_47"/>
          </VirtualSegmentString>
          <VirtualSegmentString name="SegmentGroup9">
            <PhysicalUnit name="PS_LED_48"/>
            <PhysicalUnit name="PS_LED_49"/>
            <PhysicalUnit name="PS_LED_50"/>
            <PhysicalUnit name="PS_LED_51"/>
            <PhysicalUnit name="PS_LED_52"/>
            <PhysicalUnit name="PS_LED_53"/>
            <PhysicalUnit name="PS_LED_54"/>
          </VirtualSegmentString>
          <VirtualSegmentString name="SegmentGroup10">
            <PhysicalUnit name="PS_LED_55"/>
            <PhysicalUnit name="PS_LED_56"/>
            <PhysicalUnit name="PS_LED_57"/>
            <PhysicalUnit name="PS_LED_58"/>
            <PhysicalUnit name="PS_LED_59"/>
            <PhysicalUnit name="PS_LED_60"/>
            <PhysicalUnit name="PS_LED_61"/>
          </VirtualSegmentString>
        </VirtualSegments> 
      </LEDs>  
      <PhysicalAnimations> 
        <!-- SE -->  
        <Unit name="ProgramRunningScreen_1" hwId="0x03"/>  
        <!-- BO -->  
        <Unit name="ProgramRunningScreen_2" hwId="0x08"/>  
        <!-- GA -->  
        <!-- Unit name="ProgramRunningScreen_3" hwId="0x0D"/ --> 
      </PhysicalAnimations> 
    </OutputUnits>  
    <SliderModes> 
      <SliderMode name="AbsolutMode"> 
        <!-- areas must be recalculated -->  
        <Progress name="PS_SliPos_01" TouchAreaBegin="0x00" TouchAreaEnd="0x0D"/>  
        <Progress name="PS_SliPos_02" TouchAreaBegin="0x0D" TouchAreaEnd="0x13"/>  
        <Progress name="PS_SliPos_03" TouchAreaBegin="0x13" TouchAreaEnd="0x19"/>  
        <Progress name="PS_SliPos_04" TouchAreaBegin="0x19" TouchAreaEnd="0x1F"/>  
        <Progress name="PS_SliPos_05" TouchAreaBegin="0x1F" TouchAreaEnd="0x26"/>  
        <Progress name="PS_SliPos_06" TouchAreaBegin="0x26" TouchAreaEnd="0x2C"/>  
        <Progress name="PS_SliPos_07" TouchAreaBegin="0x2C" TouchAreaEnd="0x32"/>  
        <Progress name="PS_SliPos_08" TouchAreaBegin="0x32" TouchAreaEnd="0x38"/>  
        <Progress name="PS_SliPos_09" TouchAreaBegin="0x38" TouchAreaEnd="0x3F"/>  
        <Progress name="PS_SliPos_10" TouchAreaBegin="0x3F" TouchAreaEnd="0x45"/>  
        <Progress name="PS_SliPos_11" TouchAreaBegin="0x45" TouchAreaEnd="0x4B"/>  
        <Progress name="PS_SliPos_12" TouchAreaBegin="0x4B" TouchAreaEnd="0x51"/>  
        <Progress name="PS_SliPos_13" TouchAreaBegin="0x51" TouchAreaEnd="0x58"/>  
        <Progress name="PS_SliPos_14" TouchAreaBegin="0x58" TouchAreaEnd="0x64"/> 
      </SliderMode>  
      <SliderMode name="IncrementMode" step="1"/> 
    </SliderModes> 
  </UIM-Domain_IABO_PS>  
  <UIM-Domain_IABO_OS> 
    <description>MP15 Ps Washer Siemens Bosch OS</description>  
    <version major="1" minor="0" revision="0"/>  
    <GeneralData operatingModulename="LC_UIM_MP15_02_03" nodeDBus2="0x21"/>  
    <SoundModes> 
      <!--Predefined sound in IABO, do NOT change them-->  
      <SoundMode name="NoSound" id="0"/>  
      <!-- SE -->  
      <SoundMode name="SelectionNotAllowed_1" id="1"/>  
      <SoundMode name="SoundModeActivated_1" id="2"/>  
      <SoundMode name="SoundModeDeactivated_1" id="3"/>  
      <SoundMode name="SwitchOn_1" id="4"/>  
      <SoundMode name="SwitchOff_1" id="5"/>  
      <SoundMode name="SelectionDeselection_1" id="6"/>  
      <SoundMode name="Error_1" id="7"/>  
      <SoundMode name="WarningMalfunction_1" id="8"/>  
      <SoundMode name="Finish_1" id="9"/>  
      <SoundMode name="Information_1" id="10"/>  
      <SoundMode name="SnapSignalSliding_1" id="11"/>  
      <SoundMode name="ShortPress_1" id="12"/>  
      <!-- BO -->  
      <SoundMode name="SelectionNotAllowed_2" id="13"/>  
      <SoundMode name="SoundModeActivated_2" id="14"/>  
      <SoundMode name="SoundModeDeactivated_2" id="15"/>  
      <SoundMode name="SwitchOn_2" id="16"/>  
      <SoundMode name="SwitchOff_2" id="17"/>  
      <SoundMode name="SelectionDeselection_2" id="18"/>  
      <SoundMode name="Error_2" id="19"/>  
      <SoundMode name="WarningMalfunction_2" id="20"/>  
      <SoundMode name="Finish_2" id="21"/>  
      <SoundMode name="Information_2" id="22"/>  
      <SoundMode name="SnapSignalSliding_2" id="23"/>  
      <SoundMode name="ShortPress_2" id="24"/>  
      <!-- GA -->  
      <!-- SoundMode name="SelectionNotAllowed_3" id="25"/>
            <SoundMode name="SoundModeActivated_3" id="26"/>
            <SoundMode name="SoundModeDeactivated_3" id="27"/>
            <SoundMode name="SwitchOn_3" id="28"/>
            <SoundMode name="SwitchOff_3" id="29"/>
            <SoundMode name="SelectionDeselection_3" id="30"/>
            <SoundMode name="Error_3" id="31"/>
            <SoundMode name="WarningMalfunction_3" id="32"/>
            <SoundMode name="Finish_3" id="33"/>
            <SoundMode name="Information_3" id="34"/>
            <SoundMode name="SnapSignalSliding_3" id="35"/>
            <SoundMode name="ShortPress_3" id="36"/ -->  
      <!--Customer defined sound modes/-->  
      <!--SoundMode name="BeepBeep" id="37" repetitions="0" sync="1" Frequency_00="440" Duration_00="100" Volume_00="100" Frequency_01="493" Duration_01="100" Volume_01="100"/--> 
    </SoundModes>  
    <InputUnits> 
      <Buttons> 
        <MechanicalButtons/>  
        <TouchButtons> 
          <Unit name="OS_Btn_4" hwId=""/>
          <Unit name="OS_Btn_5" hwId=""/>
          <Unit name="OS_Btn_6" hwId=""/>
          <Unit name="OS_Btn_7" hwId=""/>
        </TouchButtons> 
      </Buttons> 
    </InputUnits>  
    <OutputUnits> 
      <LEDs> 
        <PhysicalLEDs> 
          <Unit name="OS_LED_62" hwId=""/>
          <Unit name="OS_LED_63" hwId=""/>
          <Unit name="OS_LED_64" hwId=""/>
          <Unit name="OS_LED_65" hwId=""/>
          <Unit name="OS_LED_66" hwId=""/>
          <Unit name="OS_LED_67" hwId=""/>
          <Unit name="OS_LED_68" hwId=""/>
          <Unit name="OS_LED_69" hwId=""/>
          <Unit name="OS_LED_70" hwId=""/>
          <Unit name="OS_LED_71" hwId=""/>
          <Unit name="OS_LED_72" hwId=""/>
          <Unit name="OS_LED_73" hwId=""/>
          <Unit name="OS_LED_74" hwId=""/>
          <Unit name="OS_LED_75" hwId=""/>
          <Unit name="OS_LED_76" hwId=""/>
          <Unit name="OS_LED_77" hwId=""/>
          <Unit name="OS_LED_78" hwId=""/>
          <Unit name="OS_LED_79" hwId=""/>
          <Unit name="OS_LED_80" hwId=""/>
          <Unit name="OS_LED_81" hwId=""/>
          <Unit name="OS_LED_82" hwId=""/>
          <Unit name="OS_LED_83" hwId=""/>
          <Unit name="OS_LED_84" hwId=""/>
          <Unit name="OS_LED_85" hwId=""/>
          <Unit name="OS_LED_86" hwId=""/>
          <Unit name="OS_LED_87" hwId=""/>
          <Unit name="OS_LED_88" hwId=""/>
          <Unit name="OS_LED_89" hwId=""/>
          <Unit name="OS_LED_90" hwId=""/>
          <Unit name="OS_LED_91" hwId=""/>
          <Unit name="OS_LED_92" hwId=""/>
          <Unit name="OS_LED_93" hwId=""/>
          <Unit name="OS_LED_94" hwId=""/>
          <Unit name="OS_LED_95" hwId=""/>
          <Unit name="OS_LED_96" hwId=""/>
          <Unit name="OS_LED_97" hwId=""/>
          <Unit name="OS_LED_98" hwId=""/>
          <Unit name="OS_LED_99" hwId=""/>
          <Unit name="OS_LED_100" hwId=""/>
          <Unit name="OS_LED_101" hwId=""/>
          <Unit name="OS_LED_102" hwId=""/>
          <Unit name="OS_LED_103" hwId=""/>
          <Unit name="OS_LED_104" hwId=""/>
          <Unit name="OS_LED_105" hwId=""/>
          <Unit name="OS_LED_106" hwId=""/>
          <Unit name="OS_LED_107" hwId=""/>
          <Unit name="OS_LED_108" hwId=""/>
          <Unit name="OS_LED_109" hwId=""/>
          <Unit name="OS_LED_110" hwId=""/>
          <Unit name="OS_LED_111" hwId=""/>
          <Unit name="OS_LED_112" hwId=""/>
          <Unit name="OS_LED_113" hwId=""/>
          <Unit name="OS_LED_114" hwId=""/>
          <Unit name="OS_LED_115" hwId=""/>
          <Unit name="OS_LED_116" hwId=""/>
          <Unit name="OS_LED_117" hwId=""/>
          <Unit name="OS_LED_118" hwId=""/>
          <Unit name="OS_LED_119" hwId=""/>
          <Unit name="OS_LED_120" hwId=""/>
          <Unit name="OS_LED_121" hwId=""/>
          <Unit name="OS_LED_122" hwId=""/>
          <Unit name="OS_LED_123" hwId=""/>
          <Unit name="OS_LED_124" hwId=""/>
          <Unit name="OS_LED_125" hwId=""/>
          <Unit name="OS_LED_126" hwId=""/>
          <Unit name="OS_LED_127" hwId=""/>
          <Unit name="OS_LED_128" hwId=""/>
          <Unit name="OS_LED_129" hwId=""/>
          <Unit name="OS_LED_130" hwId=""/>
          <Unit name="OS_LED_131" hwId=""/>
          <Unit name="OS_LED_132" hwId=""/>
          <Unit name="OS_LED_133" hwId=""/>
          <Unit name="OS_LED_134" hwId=""/>
          <Unit name="OS_LED_135" hwId=""/>
          <Unit name="OS_LED_136" hwId=""/>
          <Unit name="OS_LED_137" hwId=""/>
          <Unit name="OS_LED_138" hwId=""/>
          <Unit name="OS_LED_139" hwId=""/>
          <Unit name="OS_LED_140" hwId=""/>
          <Unit name="OS_LED_141" hwId=""/>
          <Unit name="OS_LED_142" hwId=""/>
          <Unit name="OS_LED_143" hwId=""/>
          <Unit name="OS_LED_144" hwId=""/>
          <Unit name="OS_LED_145" hwId=""/>
          <Unit name="OS_LED_146" hwId=""/>
          <Unit name="OS_LED_147" hwId=""/>
          <Unit name="OS_LED_148" hwId=""/>
          <Unit name="OS_LED_149" hwId=""/>
          <Unit name="OS_LED_150" hwId=""/>
          <Unit name="OS_LED_151" hwId=""/>
          <Unit name="OS_LED_152" hwId=""/>
          <Unit name="OS_LED_153" hwId=""/>
          <Unit name="OS_LED_154" hwId=""/>
          <Unit name="OS_LED_155" hwId=""/>
          <Unit name="OS_LED_156" hwId=""/>
          <Unit name="OS_LED_157" hwId=""/>
          <Unit name="OS_LED_158" hwId=""/>
          <Unit name="OS_LED_159" hwId=""/>
          <Unit name="OS_LED_160" hwId=""/>
          <Unit name="OS_LED_161" hwId=""/>
          <Unit name="OS_LED_162" hwId=""/>
          <Unit name="OS_LED_163" hwId=""/>
          <Unit name="OS_LED_164" hwId=""/>
          <Unit name="OS_LED_165" hwId=""/>
          <Unit name="OS_LED_166" hwId=""/>
          <Unit name="OS_LED_167" hwId=""/>
          <Unit name="OS_LED_168" hwId=""/>
          <Unit name="OS_LED_169" hwId=""/>
          <Unit name="OS_LED_170" hwId=""/>
          <Unit name="OS_LED_171" hwId=""/>
          <Unit name="OS_LED_172" hwId=""/>
          <Unit name="OS_LED_173" hwId=""/>
        </PhysicalLEDs>  
        <VirtualSegments> 
          <VirtualSegment name="SegmentGroup11">
            <PhysicalUnit name="OS_LED_62"/>
            <PhysicalUnit name="OS_LED_63"/>
            <PhysicalUnit name="OS_LED_64"/>
            <PhysicalUnit name="OS_LED_65"/>
            <PhysicalUnit name="OS_LED_66"/>
            <PhysicalUnit name="OS_LED_67"/>
            <PhysicalUnit name="OS_LED_68"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup12">
            <PhysicalUnit name="OS_LED_69"/>
            <PhysicalUnit name="OS_LED_70"/>
            <PhysicalUnit name="OS_LED_71"/>
            <PhysicalUnit name="OS_LED_72"/>
            <PhysicalUnit name="OS_LED_73"/>
            <PhysicalUnit name="OS_LED_74"/>
            <PhysicalUnit name="OS_LED_75"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup13">
            <PhysicalUnit name="OS_LED_76"/>
            <PhysicalUnit name="OS_LED_77"/>
            <PhysicalUnit name="OS_LED_78"/>
            <PhysicalUnit name="OS_LED_79"/>
            <PhysicalUnit name="OS_LED_80"/>
            <PhysicalUnit name="OS_LED_81"/>
            <PhysicalUnit name="OS_LED_82"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup14">
            <PhysicalUnit name="OS_LED_83"/>
            <PhysicalUnit name="OS_LED_84"/>
            <PhysicalUnit name="OS_LED_85"/>
            <PhysicalUnit name="OS_LED_86"/>
            <PhysicalUnit name="OS_LED_87"/>
            <PhysicalUnit name="OS_LED_88"/>
            <PhysicalUnit name="OS_LED_89"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup15">
            <PhysicalUnit name="OS_LED_90"/>
            <PhysicalUnit name="OS_LED_91"/>
            <PhysicalUnit name="OS_LED_92"/>
            <PhysicalUnit name="OS_LED_93"/>
            <PhysicalUnit name="OS_LED_94"/>
            <PhysicalUnit name="OS_LED_95"/>
            <PhysicalUnit name="OS_LED_96"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup16">
            <PhysicalUnit name="OS_LED_100"/>
            <PhysicalUnit name="OS_LED_101"/>
            <PhysicalUnit name="OS_LED_102"/>
            <PhysicalUnit name="OS_LED_103"/>
            <PhysicalUnit name="OS_LED_97"/>
            <PhysicalUnit name="OS_LED_98"/>
            <PhysicalUnit name="OS_LED_99"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup17">
            <PhysicalUnit name="OS_LED_104"/>
            <PhysicalUnit name="OS_LED_105"/>
            <PhysicalUnit name="OS_LED_106"/>
            <PhysicalUnit name="OS_LED_107"/>
            <PhysicalUnit name="OS_LED_108"/>
            <PhysicalUnit name="OS_LED_109"/>
            <PhysicalUnit name="OS_LED_110"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup18">
            <PhysicalUnit name="OS_LED_111"/>
            <PhysicalUnit name="OS_LED_112"/>
            <PhysicalUnit name="OS_LED_113"/>
            <PhysicalUnit name="OS_LED_114"/>
            <PhysicalUnit name="OS_LED_115"/>
            <PhysicalUnit name="OS_LED_116"/>
            <PhysicalUnit name="OS_LED_117"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup19">
            <PhysicalUnit name="OS_LED_118"/>
            <PhysicalUnit name="OS_LED_119"/>
            <PhysicalUnit name="OS_LED_120"/>
            <PhysicalUnit name="OS_LED_121"/>
            <PhysicalUnit name="OS_LED_122"/>
            <PhysicalUnit name="OS_LED_123"/>
            <PhysicalUnit name="OS_LED_124"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup20">
            <PhysicalUnit name="OS_LED_125"/>
            <PhysicalUnit name="OS_LED_126"/>
            <PhysicalUnit name="OS_LED_127"/>
            <PhysicalUnit name="OS_LED_128"/>
            <PhysicalUnit name="OS_LED_129"/>
            <PhysicalUnit name="OS_LED_130"/>
            <PhysicalUnit name="OS_LED_131"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup21">
            <PhysicalUnit name="OS_LED_132"/>
            <PhysicalUnit name="OS_LED_133"/>
            <PhysicalUnit name="OS_LED_134"/>
            <PhysicalUnit name="OS_LED_135"/>
            <PhysicalUnit name="OS_LED_136"/>
            <PhysicalUnit name="OS_LED_137"/>
            <PhysicalUnit name="OS_LED_138"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup22">
            <PhysicalUnit name="OS_LED_139"/>
            <PhysicalUnit name="OS_LED_140"/>
            <PhysicalUnit name="OS_LED_141"/>
            <PhysicalUnit name="OS_LED_142"/>
            <PhysicalUnit name="OS_LED_143"/>
            <PhysicalUnit name="OS_LED_144"/>
            <PhysicalUnit name="OS_LED_145"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup23">
            <PhysicalUnit name="OS_LED_146"/>
            <PhysicalUnit name="OS_LED_147"/>
            <PhysicalUnit name="OS_LED_148"/>
            <PhysicalUnit name="OS_LED_149"/>
            <PhysicalUnit name="OS_LED_150"/>
            <PhysicalUnit name="OS_LED_151"/>
            <PhysicalUnit name="OS_LED_152"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup24">
            <PhysicalUnit name="OS_LED_153"/>
            <PhysicalUnit name="OS_LED_154"/>
            <PhysicalUnit name="OS_LED_155"/>
            <PhysicalUnit name="OS_LED_156"/>
            <PhysicalUnit name="OS_LED_157"/>
            <PhysicalUnit name="OS_LED_158"/>
            <PhysicalUnit name="OS_LED_159"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup25">
            <PhysicalUnit name="OS_LED_160"/>
            <PhysicalUnit name="OS_LED_161"/>
            <PhysicalUnit name="OS_LED_162"/>
            <PhysicalUnit name="OS_LED_163"/>
            <PhysicalUnit name="OS_LED_164"/>
            <PhysicalUnit name="OS_LED_165"/>
            <PhysicalUnit name="OS_LED_166"/>
          </VirtualSegment>
          <VirtualSegment name="SegmentGroup26">
            <PhysicalUnit name="OS_LED_167"/>
            <PhysicalUnit name="OS_LED_168"/>
            <PhysicalUnit name="OS_LED_169"/>
            <PhysicalUnit name="OS_LED_170"/>
            <PhysicalUnit name="OS_LED_171"/>
            <PhysicalUnit name="OS_LED_172"/>
            <PhysicalUnit name="OS_LED_173"/>
          </VirtualSegment>
          <VirtualSegmentString name="S">
            <VirtualUnit name="SegmentGroup11"/>
            <VirtualUnit name="SegmentGroup12"/>
            <VirtualUnit name="SegmentGroup13"/>
            <VirtualUnit name="SegmentGroup14"/>
            <VirtualUnit name="SegmentGroup15"/>
            <VirtualUnit name="SegmentGroup16"/>
            <VirtualUnit name="SegmentGroup17"/>
            <VirtualUnit name="SegmentGroup18"/>
            <VirtualUnit name="SegmentGroup19"/>
            <VirtualUnit name="SegmentGroup20"/>
            <VirtualUnit name="SegmentGroup21"/>
            <VirtualUnit name="SegmentGroup22"/>
            <VirtualUnit name="SegmentGroup23"/>
            <VirtualUnit name="SegmentGroup24"/>
            <VirtualUnit name="SegmentGroup25"/>
            <VirtualUnit name="SegmentGroup26"/>
          </VirtualSegmentString>
        </VirtualSegments> 
      </LEDs>  
      <PhysicalAnimations> 
        <!-- SE -->  
        <Unit name="ProcessOngoing_1" hwId="0x04"/>  
        <!-- BO -->  
        <Unit name="ProcessOngoing_2" hwId="0x08"/>  
        <!-- GA -->  
        <!-- Unit name="ProcessOngoing_3" hwId="0x0E"/ --> 
      </PhysicalAnimations> 
    </OutputUnits> 
  </UIM-Domain_IABO_OS>  
  <UIM-Domain_only> 
    <description>MP15 Ps Washer Bosch</description>  
    <version major="1" minor="0" revision="0"/>  
    <InputUnits> 
      <!-- Maps a fixed or variable logical input to a physical button-->  
      <Buttons> 
        <ButtonFunctions></ButtonFunctions> 
      </Buttons>  
      <Encoders> 
        <Slider> 
          <SliderFunctions> 
            <SliderMapping name="ProgramSelector01" unitname="PS_SliPos_01"/>  
            <SliderMapping name="ProgramSelector02" unitname="PS_SliPos_02"/>  
            <SliderMapping name="ProgramSelector03" unitname="PS_SliPos_03"/>  
            <SliderMapping name="ProgramSelector04" unitname="PS_SliPos_04"/>  
            <SliderMapping name="ProgramSelector05" unitname="PS_SliPos_05"/>  
            <SliderMapping name="ProgramSelector06" unitname="PS_SliPos_06"/>  
            <SliderMapping name="ProgramSelector07" unitname="PS_SliPos_07"/>  
            <SliderMapping name="ProgramSelector08" unitname="PS_SliPos_08"/>  
            <SliderMapping name="ProgramSelector09" unitname="PS_SliPos_09"/>  
            <SliderMapping name="ProgramSelector10" unitname="PS_SliPos_10"/>  
            <SliderMapping name="ProgramSelector11" unitname="PS_SliPos_11"/>  
            <SliderMapping name="ProgramSelector12" unitname="PS_SliPos_12"/>  
            <SliderMapping name="ProgramSelector13" unitname="PS_SliPos_13"/>  
            <SliderMapping name="ProgramSelector14" unitname="PS_SliPos_14"/> 
          </SliderFunctions> 
        </Slider> 
      </Encoders>  
      <Layouts> 
        <Layout name="ProgramsLayout"> 
          <!--To be removed after UI Logic app read from domain interface/-->  
          <LayoutItem name="ProgramSelector01"/>  
          <LayoutItem name="ProgramSelector02"/>  
          <LayoutItem name="ProgramSelector03"/>  
          <LayoutItem name="ProgramSelector04"/>  
          <LayoutItem name="ProgramSelector05"/>  
          <LayoutItem name="ProgramSelector06"/>  
          <LayoutItem name="ProgramSelector07"/>  
          <LayoutItem name="ProgramSelector08"/>  
          <LayoutItem name="ProgramSelector09"/>  
          <LayoutItem name="ProgramSelector10"/>  
          <LayoutItem name="ProgramSelector11"/>  
          <LayoutItem name="ProgramSelector12"/>  
          <LayoutItem name="ProgramSelector13"/>  
          <LayoutItem name="ProgramSelector14"/> 
        </Layout>  
        <Layout name="StainsLayout"> 
          <LayoutItem name="Position06"/>  
          <LayoutItem name="Position05"/>  
          <LayoutItem name="Position04"/>  
          <LayoutItem name="Position03"/>  
          <LayoutItem name="Position02"/>  
          <LayoutItem name="Position01"/> 
        </Layout>  
        <Layout name="MoreLayout"> 
          <LayoutItem name="VariableFunctionKey01"/>  
          <LayoutItem name="VariableFunctionKey02"/>  
          <LayoutItem name="VariableFunctionKey03"/>  
          <LayoutItem name="VariableFunctionKey04"/>  
          <LayoutItem name="BasicSetup.toggle"/>  
          <LayoutItem name="ChildLock.toggle"/> 
        </Layout>  
        <!-- Layout name="ProgramsLayout" --> 
      </Layouts> 
    </InputUnits>  
    <OutputUnits> 
      <!-- Maps a fixed or variable function to a physical output -->  
      <LEDs> 
        <LEDFunctionMappingSimple></LEDFunctionMappingSimple>  
        <LEDFunctionMappingExtended></LEDFunctionMappingExtended>  
        <LEDFunctionMappingVariable> 
          <!-- VariableFunctions -->  
          <LEDFunctionVariable name="Prewash.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="WaterPlus.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="RinsePlus1.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="Steam.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="Memory"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="MiniLoad.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="Soak.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="RinsePlus3.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="WaterAndRinsePlus1.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="IDos1Active.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/>  
              <!-- implizit selected with warning -->  
              <!-- LED name="Button.warning" unitname="OS_LED_119" ledMode="Flashrate_2"/ -->  
              <!-- special tag LEDFunction means that parser can take the name directly (nothing to build, no spliting) -->  
              <LEDFunction name="SymbolIDos1Detergent.Filled.selectable" unitname="OS_LED_117_W" ledMode="Dimming"/>  
              <LEDFunction name="SymbolIDos1Detergent.Filled.selected" unitname="OS_LED_117_W" ledMode="On"/>  
              <!-- hard coded -->  
              <!-- LEDFunction name="SymbolIDos1Detergent.EmptySelected" unitname="OS_LED_117_W" ledMode="Flashrate_2"/ --> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/>  
              <!-- implizit selected with warning -->  
              <!-- LED name="Button.warning" unitname="OS_LED_123" ledMode="Flashrate_2"/ -->  
              <!-- special tag LEDFunction means that parser can take the name directly (nothing to build, no spliting) -->  
              <LEDFunction name="SymbolIDos1Detergent.Filled.selectable" unitname="OS_LED_121_W" ledMode="Dimming"/>  
              <LEDFunction name="SymbolIDos1Detergent.Filled.selected" unitname="OS_LED_121_W" ledMode="On"/>  
              <!-- hard coded -->  
              <!-- LEDFunction name="SymbolIDos1Detergent.EmptySelected" unitname="OS_LED_121_W" ledMode="Flashrate_2"/ --> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="IDos2Active.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/>  
              <!-- implizit selected with warning -->  
              <!-- LED name="Button.warning" unitname="OS_LED_123" ledMode="Flashrate_2"/ -->  
              <!-- special tag LEDFunction means that parser can take the name directly (nothing to build, no spliting) -->  
              <LEDFunction name="SymbolIDos2Detergent.Filled.selectable" unitname="OS_LED_120_W" ledMode="Dimming"/>  
              <LEDFunction name="SymbolIDos2Detergent.Filled.selected" unitname="OS_LED_120_W" ledMode="On"/>  
              <LEDFunction name="SymbolIDos2Softener.Filled.selectable" unitname="OS_LED_121_W" ledMode="Dimming"/>  
              <LEDFunction name="SymbolIDos2Softener.Filled.selected" unitname="OS_LED_121_W" ledMode="On"/>  
              <!-- hard coded -->  
              <!-- LEDFunction name="SymbolIDos2Detergent.Empty.selected" unitname="OS_LED_120_W" ledMode="Flashrate_2"/ -->  
              <!-- LEDFunction name="SymbolIDos2Softener.Empty.selected" unitname="OS_LED_121_W" ledMode="Flashrate_2"/ --> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/>  
              <!-- implizit selected with warning -->  
              <!-- LED name="Button.warning" unitname="OS_LED_127" ledMode="Flashrate_2"/ -->  
              <!-- special tag LEDFunction means that parser can take the name directly (nothing to build, no spliting) -->  
              <LEDFunction name="SymbolIDos2Detergent.Filled.selectable" unitname="OS_LED_124_W" ledMode="Dimming"/>  
              <LEDFunction name="SymbolIDos2Detergent.Filled.selected" unitname="OS_LED_124_W" ledMode="On"/>  
              <LEDFunction name="SymbolIDos2Softener.Filled.selectable" unitname="OS_LED_125_W" ledMode="Dimming"/>  
              <LEDFunction name="SymbolIDos2Softener.Filled.selected" unitname="OS_LED_125_W" ledMode="On"/>  
              <!-- hard coded -->  
              <!-- LEDFunction name="SymbolIDos2Detergent.Empty.selected" unitname="OS_LED_124_W" ledMode="Flashrate_2"/ -->  
              <!-- LEDFunction name="SymbolIDos2Softener.Empty.selected" unitname="OS_LED_125_W" ledMode="Flashrate_2"/ --> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="SilentMode.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="IntensivePlus.toggle"> 
            <LEDFunctionExtended name="VariableFunctionKey01"> 
              <!-- is OP_KEY_02 -->  
              <LED name="Button.selectable" unitname="OS_LED_2" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_2" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey02"> 
              <!-- is OP_KEY_03 -->  
              <LED name="Button.selectable" unitname="OS_LED_4" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_4" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey03"> 
              <!-- is OP_KEY_04 -->  
              <LED name="Button.selectable" unitname="OS_LED_6" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_6" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey04"> 
              <!-- is OP_KEY_05 -->  
              <LED name="Button.selectable" unitname="OS_LED_8" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_8" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey06"> 
              <!-- is OP_KEY_09 -->  
              <LED name="Button.selectable" unitname="OS_LED_119" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_119" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey07"> 
              <!-- is OP_KEY_10 -->  
              <LED name="Button.selectable" unitname="OS_LED_123" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_123" ledMode="On"/> 
            </LEDFunctionExtended>  
            <LEDFunctionExtended name="VariableFunctionKey08"> 
              <!-- is OP_KEY_011 -->  
              <LED name="Button.selectable" unitname="OS_LED_127" ledMode="On"/>  
              <LED name="Button.selected" unitname="OS_LED_127" ledMode="On"/>  
              <LED name="Symbol.selected" unitname="OS_LED_125_B" ledMode="On"/> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable>  
          <LEDFunctionVariable name="Stains.next"> 
            <LEDFunctionExtended name="VariableFunctionKey05"> 
              <!-- is OP_KEY_08 -->  
              <LED name="Button.selectable" unitname="OS_LED_20_R" ledMode="Dimming"/>  
              <LED name="Button.selected" unitname="OS_LED_20_R" ledMode="On"/>  
              <!-- no specific symbol in this case -->  
              <!-- LED name="Symbol.selected" unitname="OS_LED_19_B" ledMode="On"/ --> 
            </LEDFunctionExtended> 
          </LEDFunctionVariable> 
        </LEDFunctionMappingVariable>  
        <LEDFunctionMappingLayout layoutname="StainsLayout"> 
          <!-- both lines moved to variable functions stains.next -->  
          <!-- LED name="Button.selectable" unitname="OS_LED_20_W" ledMode="On"/ -->  
          <!-- LED name="Button.selected" unitname="OS_LED_20_W" ledMode="On"/ -->  
          <!-- is OP_KEY_08 -->  
          <LEDFunctionExtended name="Position01"> 
            <LED name="Symbol.selectable" unitname="OS_LED_19_R" ledMode="Dimming"/>  
            <LED name="Symbol.selected" unitname="OS_LED_19_R" ledMode="On"/> 
          </LEDFunctionExtended>  
          <LEDFunctionExtended name="Position02"> 
            <LED name="Symbol.selectable" unitname="OS_LED_18_R" ledMode="Dimming"/>  
            <LED name="Symbol.selected" unitname="OS_LED_18_R" ledMode="On"/> 
          </LEDFunctionExtended>  
          <LEDFunctionExtended name="Position03"> 
            <LED name="Symbol.selectable" unitname="OS_LED_17_R" ledMode="Dimming"/>  
            <LED name="Symbol.selected" unitname="OS_LED_17_R" ledMode="On"/> 
          </LEDFunctionExtended>  
          <LEDFunctionExtended name="Position04"> 
            <LED name="Symbol.selectable" unitname="OS_LED_16_R" ledMode="Dimming"/>  
            <LED name="Symbol.selected" unitname="OS_LED_16_R" ledMode="On"/> 
          </LEDFunctionExtended>  
          <LEDFunctionExtended name="Position05"> 
            <LED name="Symbol.selectable" unitname="OS_LED_15_R" ledMode="Dimming"/>  
            <LED name="Symbol.selected" unitname="OS_LED_15_R" ledMode="On"/> 
          </LEDFunctionExtended>  
          <LEDFunctionExtended name="Position06"> 
            <LED name="Symbol.selectable" unitname="OS_LED_14_R" ledMode="Dimming"/>  
            <LED name="Symbol.selected" unitname="OS_LED_14_R" ledMode="On"/> 
          </LEDFunctionExtended> 
        </LEDFunctionMappingLayout> 
      </LEDs>  
      <Sounds> 
        <SoundFunctionMapping> 
          <!-- BO -->  
          <SoundFunction name="SelectionNotAllowed" soundmode="SelectionNotAllowed_2"/>  
          <SoundFunction name="SoundModeActivated" soundmode="SoundModeActivated_2"/>  
          <SoundFunction name="SoundModeDeactivated" soundmode="SoundModeDeactivated_2"/>  
          <SoundFunction name="SwitchOn" soundmode="SwitchOn_2"/>  
          <SoundFunction name="SwitchOff" soundmode="SwitchOff_2"/>  
          <SoundFunction name="SelectionDeselection" soundmode="SelectionDeselection_2"/>  
          <SoundFunction name="Error" soundmode="Error_2"/>  
          <SoundFunction name="WarningMalfunction" soundmode="WarningMalfunction_2"/>  
          <SoundFunction name="Finish" soundmode="Finish_2"/>  
          <SoundFunction name="Information" soundmode="Information_2"/>  
          <SoundFunction name="SnapSignalSliding" soundmode="SnapSignalSliding_2"/>  
          <SoundFunction name="ShortPress" soundmode="ShortPress_2"/> 
        </SoundFunctionMapping> 
      </Sounds>  
      <Animations> 
        <!-- BO -->  
        <AnimationFunction name="StartUpAttraction" unitname="StartUpAttraction_2"/>  
        <AnimationFunction name="ProcessOngoing" unitname="ProcessOngoing_2"/>  
        <AnimationFunction name="ProgramRunningScreen" unitname="ProgramRunningScreen_2"/>  
        <!-- BO
                <AnimationFunction name="FunctionPhase" unitname="FunctionPhase_2"/>
                <AnimationFunction name="FirmwareUpdate" unitname="FirmwareUpdate_2"/>--> 
      </Animations> 
    </OutputUnits> 
  </UIM-Domain_only> 
</IABO_Config>

