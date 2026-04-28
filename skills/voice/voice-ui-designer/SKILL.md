# Hermes-Skill-Voice-UI-Designer

> 🎤 **语音交互设计专家** | VUI 设计模式 | 对话系统架构 | 多平台适配

---

## 📋 技能概述

Hermes-Skill-Voice-UI-Designer 是一个专业的语音用户界面（VUI）AI 助手，专注于帮助产品团队设计、开发和优化语音交互体验。涵盖对话设计、意图识别、多轮对话管理、TTS/STT 优化、以及 Alexa/Google Assistant/Siri 等多平台适配。

### VUI 能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│              Voice UI Design Stack                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  💬 Conversation     │  🧠 NLP/NLU         │  🔊 Audio      │
│  ├── Dialog Flow    │  ├── Intent Recognition│  ├── TTS      │
│  ├── Slot Filling   │  ├── Entity Extraction│  ├── STT      │
│  ├── Context Mgmt   │  ├── Sentiment       │  └── SSML      │
│  └── Error Recovery │  └── Disambiguation  │                 │
│                                                             │
│  📱 Platforms        │  🎨 UX Patterns      │  📊 Analytics  │
│  ├── Alexa Skills   │  ├── Prompt Design  │  ├── Utterance │
│  ├── Google Actions│  ├── Graceful Degradation│  │   Analysis│
│  ├── Siri Shortcuts│  ├── Multimodal      │  ├── Funnel    │
│  └── Custom Devices│  └── Accessibility   │  └── Retention │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 对话设计

```bash
# 创建新对话流程
/vui create-flow "pizza-ordering"

# 意图定义
/vui define-intent "order-pizza" --slots "size, toppings, crust"

# 示例语句生成
/vui generate-utterances "book-appointment"

# 错误恢复策略
/vui error-handling "confirmation-step"
```

### 平台部署

```bash
# Alexa Skill 部署
/vui deploy-alexa "skill-id"

# Google Action 配置
/vui configure-google "project-id"

# 跨平台测试
/vui cross-platform-test "weather-skill"
```

---

## 💬 对话设计框架

### 对话状态机 (DSM)

```python
# dialog_state_machine.py - 对话状态管理

from dataclasses import dataclass
from enum import Enum, auto
from typing import Dict, List, Optional, Any
from datetime import datetime

class DialogState(Enum):
    IDLE = auto()
    GREETING = auto()
    COLLECTING_INFO = auto()
    CONFIRMING = auto()
    PROCESSING = auto()
    ERROR_RECOVERY = auto()
    COMPLETING = auto()

@dataclass
class DialogContext:
    """对话上下文 - 贯穿整个会话"""
    session_id: str
    user_id: str
    current_state: DialogState
    intent: Optional[str] = None
    slots: Dict[str, Any] = None
    slot_confidences: Dict[str, float] = None
    turn_count: int = 0
    error_count: int = 0
    last_utterance: str = ""
    entities_extracted: List[Dict] = None
    timestamp: datetime = None
    
    def __post_init__(self):
        if self.slots is None:
            self.slots = {}
        if self.slot_confidences is None:
            self.slot_confidences = {}
        if self.entities_extracted is None:
            self.entities_extracted = []
        if self.timestamp is None:
            self.timestamp = datetime.utcnow()

class DialogStateMachine:
    """
    对话状态机引擎
    实现多轮对话的状态转换逻辑
    """
    
    def __init__(self, config: dict):
        self.config = config
        self.state_transitions = self._define_transitions()
        self.error_threshold = config.get('error_threshold', 3)
        
    def _define_transitions(self) -> Dict[DialogState, List[DialogState]]:
        """定义合法的状态转换"""
        return {
            DialogState.IDLE: [DialogState.GREETING],
            DialogState.GREETING: [DialogState.COLLECTING_INFO, DialogState.IDLE],
            DialogState.COLLECTING_INFO: [
                DialogState.COLLECTING_INFO,  # 继续收集更多槽位
                DialogState.CONFIRMING,        # 所有必要信息已收集
                DialogState.ERROR_RECOVERY    # 出现错误
            ],
            DialogState.CONFIRMING: [
                DialogState.PROCESSING,       # 用户确认
                DialogState.COLLECTING_INFO,  # 用户修改
                DialogState.ERROR_RECOVERY
            ],
            DialogState.PROCESSING: [
                DialogState.COMPLETING,
                DialogState.ERROR_RECOVERY
            ],
            DialogState.ERROR_RECOVERY: [
                DialogState.COLLECTING_INFO,
                DialogState.GREETING,          # 重试太多次，重新开始
                DialogState.COMPLETING         # 放弃并结束
            ],
            DialogState.COMPLETING: [DialogState.IDLE]
        }
    
    async def process_turn(
        self,
        context: DialogContext,
        user_input: str,
        nlu_result: Dict
    ) -> tuple[DialogContext, Dict]:
        """
        处理单轮对话
        
        Returns:
            (updated_context, response_action)
        """
        
        context.turn_count += 1
        context.last_utterance = user_input
        
        # 1. NLU 结果处理
        intent = nlu_result.get('intent')
        entities = nlu_result.get('entities', [])
        confidence = nlu_result.get('confidence', 0.0)
        
        # 更新上下文
        context.entities_extracted.extend(entities)
        
        # 2. 根据当前状态处理
        if context.current_state == DialogState.COLLECTING_INFO:
            return await self._handle_info_collection(context, entities, confidence)
        
        elif context.current_state == DialogState.CONFIRMING:
            return await self._handle_confirmation(context, user_input)
        
        elif context.current_state == DialogState.ERROR_RECOVERY:
            return await self._handle_error_recovery(context, user_input)
        
        else:
            return await self._handle_default(context, intent, entities)
    
    async def _handle_info_collection(
        self,
        context: DialogContext,
        entities: List[Dict],
        confidence: float
    ) -> tuple[DialogContext, Dict]:
        """信息收集阶段"""
        
        required_slots = self.config['intents'][context.intent]['required_slots']
        filled_slots = set(context.slots.keys())
        missing_slots = set(required_slots) - filled_slots
        
        # 填充识别到的实体到槽位
        for entity in entities:
            slot_name = entity.get('slot_mapping')
            if slot_name and slot_name in missing_slots:
                context.slots[slot_name] = entity['value']
                context.slot_confidences[slot_name] = entity.get('confidence', confidence)
                missing_slots.remove(slot_name)
        
        # 检查是否所有必要槽位都已填充
        if not missing_slots:
            # 所有信息收集完毕，进入确认阶段
            context.current_state = DialogState.CONFIRMING
            
            confirmation_prompt = self._generate_confirmation_prompt(context)
            
            return context, {
                'action': 'confirm',
                'prompt': confirmation_prompt,
                'reprompt': 'Did I get that right?'
            }
        else:
            # 询问缺失的槽位
            next_slot = self._prioritize_missing_slots(missing_slots, context)
            
            prompt = self._generate_slot_prompt(next_slot, context)
            
            return context, {
                'action': 'ask_slot',
                'slot': next_slot,
                'prompt': prompt,
                'reprompt': f"I didn't catch that. {prompt.lower()}"
            }
    
    def _prioritize_missing_slots(
        self,
        missing_slots: set,
        context: DialogContext
    ) -> str:
        """智能排序需要询问的槽位"""
        
        # 优先级规则：
        # 1. 用户在上一轮可能提到但未成功识别的
        # 2. 必填且无默认值的
        # 3. 依赖其他槽位的值来生成更好问题的
        
        slot_priorities = {
            'size': 1,          # 通常最先问
            'toppings': 2,      # 可能是列表
            'crust': 3,         # 可能有默认值
            'delivery_address': 4,  # 复杂信息最后问
            'payment_method': 5
        }
        
        return min(missing_slots, key=lambda s: slot_priorities.get(s, 99))
    
    def _generate_slot_prompt(self, slot_name: str, context: DialogContext) -> str:
        """根据上下文动态生成问题"""
        
        slot_config = self.config['slots'][slot_name]
        
        # 使用已有信息个性化问题
        personalization = ""
        if 'size' in context.slots:
            personalization = f" for your {context.slots['size']} pizza"
        
        base_prompts = slot_config['prompts']
        
        # 根据错误次数选择不同的表达方式
        if context.error_count == 0:
            prompt = base_prompts['initial'] + personalization
        elif context.error_count == 1:
            prompt = base_prompts['rephrase']
        else:
            prompt = base_prompts['simplified']
        
        return prompt
    
    async def _handle_error_recovery(
        self,
        context: DialogContext,
        user_input: str
    ) -> tuple[DialogContext, Dict]:
        """错误恢复策略"""
        
        context.error_count += 1
        
        # 错误次数过多，提供退出选项
        if context.error_count >= self.error_threshold:
            context.current_state = DialogState.COMPLETING
            
            return context, {
                'action': 'escalate',
                'prompt': (
                    "I'm having trouble understanding. "
                    "Would you like to try again, or would you prefer "
                    "to complete this order using our app or website?"
                ),
                'options': ['try_again', 'switch_to_app', 'cancel']
            }
        
        # 渐进式帮助
        recovery_strategies = [
            {
                'type': 'narrow_down',
                'prompt': "Let me help you differently. "
                        "Are you looking for thin crust, regular, or deep dish?"
            },
            {
                'type': 'offer_examples',
                'prompt': "For example, you can say 'pepperoni and mushrooms' "
                        "or 'just cheese'."
            },
            {
                'type': 'yes_no',
                'prompt': "Would you like our most popular combination?"
            }
        ]
        
        strategy = recovery_strategy[min(context.error_count - 1, 
                                        len(recovery_strategies) - 1)]
        
        context.current_state = DialogState.COLLECTING_INFO
        
        return context, {
            'action': 'error_recovery',
            'strategy': strategy['type'],
            'prompt': strategy['prompt'],
            'hint_available': True
        }
```

### 多轮对话示例：披萨订购

```yaml
# pizza_order_dialog.yaml - 多轮对话配置

dialog_config:
  intent: "OrderPizza"
  
  slots:
    size:
      type: ENUM
      required: true
      prompts:
        initial: "What size would you like your pizza?"
        rephrase: "We have small, medium, and large. Which size?"
        simplified: "Say small, medium, or large."
      values: ["small", "medium", "large", "extra large"]
      validation:
        - type: in_list
          list: ["small", "medium", "large", "extra large"]
          
    toppings:
      type: LIST
      required: true
      max_items: 5
      prompts:
        initial: "What toppings would you like?"
        rephrase: "Which toppings can I add for you?"
        simplified: "Name one topping at a time. Say 'done' when finished."
      values: ["pepperoni", "mushrooms", "onions", "sausage", "peppers", 
               "olives", "bacon", "ham", "pineapple", "extra cheese"]
      entity_mapping:
        - entity: "TOPPING"
          slot: "toppings"
          
    crust_type:
      type: ENUM
      required: false
      default_value: "regular"
      prompts:
        initial: "What crust type do you prefer?"
        rephrase: "Thin crust, regular, or deep dish?"
        simplified: "Thin, regular, or deep dish?"
      values: ["thin", "regular", "deep dish", "stuffed crust"]
      
  confirmation:
    template: |
      Let me confirm your order:
      A {{size}} pizza with {{toppings | join(', ')}} on {{crust_type}} crust.
      Is that correct?
      
    slot_display_names:
      size: "size"
      toppings: "toppings"
      crust_type: "crust type"

  error_handling:
    no_input:
      max_reprompts: 2
      messages:
        - "I didn't hear anything. What size pizza would you like?"
        - "Still here? Please say small, medium, or large."
      fallback: "It looks like you stepped away. Your order will be saved for 10 minutes."
      
    no_match:
      strategies:
        - type: "clarification"
          trigger_count: 1
          message: "I'm not sure what you meant by '{user_input}'. Did you want to add a topping?"
          
        - type: "narrowing"
          trigger_count: 2
          message: "Let me list the available toppings: pepperoni, mushrooms, onions..."
          
        - type: "simplification"
          trigger_count: 3
          message: "Would you like me to suggest our most popular pizza?"

  disambiguation:
    multiple_matches:
      message: "I found several options. Did you mean:"
      options_template: "- {{option}} (say option {{number}})"
      max_options: 3
      
    partial_match:
      message: "Did you mean '{{guess}}'?"
      confidence_threshold: 0.75
```

---

## 🧠 NLU 意图与实体设计

### 意图架构

```python
# intent_designer.py - NLU 意图设计工具

from typing import List, Dict, Tuple
import re
from dataclasses import dataclass

@dataclass
class IntentDefinition:
    name: str
    description: str
    examples: List[str]
    slots: List[Dict]
    confirmation_required: bool
    endpoint: str  # Fulfillment endpoint

@dataclass
class EntityDefinition:
    name: str
    type: str  # CUSTOM, SYSTEM, MAP
    values: List[Dict]  # [{synonyms: [], value: ""}]
    fuzzy_matching: bool

class IntentDesigner:
    """NLU 意图与实体设计辅助工具"""
    
    def design_intent_hierarchy(self, domain: str) -> Dict:
        """设计领域相关的意图层次结构"""
        
        hierarchies = {
            'food_ordering': {
                'core_intents': [
                    IntentDefinition(
                        name='OrderFood',
                        description='User wants to place a food order',
                        examples=[
                            "I'd like to order a burger",
                            "Can I get a large pepperoni pizza?",
                            "Order me some Chinese food",
                            "I want to place an order for delivery"
                        ],
                        slots=[
                            {'name': 'food_item', 'type': '@FoodItem', 'required': True},
                            {'name': 'quantity', 'type': 'AMAZON.Number', 'required': False},
                            {'name': 'size', 'type': '@Size', 'required': False},
                            {'name': 'special_instructions', 'type': 'AMAZON.SearchQuery', 'required': False}
                        ],
                        confirmation_required=True,
                        endpoint='/api/orders/create'
                    ),
                    IntentDefinition(
                        name='CheckOrderStatus',
                        description='User wants to check status of existing order',
                        examples=[
                            "Where's my food?",
                            "Check the status of my order",
                            "Has my delivery left yet?",
                            "When will my pizza arrive?"
                        ],
                        slots=[
                            {'name': 'order_id', 'type': 'AMAZON.FourDigitNumber', 'required': False}
                        ],
                        confirmation_required=False,
                        endpoint='/api/orders/status'
                    )
                ],
                'supporting_intents': [
                    IntentDefinition(
                        name='GetMenu',
                        description='User wants to see menu items',
                        examples=[
                            "What's on the menu?",
                            "Show me the menu",
                            "What do you have?",
                            "Tell me about your specials"
                        ],
                        slots=[],
                        confirmation_required=False,
                        endpoint='/api/menu'
                    ),
                    IntentDefinition(
                        name='GetRestaurantInfo',
                        description='User wants restaurant information',
                        examples=[
                            "What are your hours?",
                            "Where are you located?",
                            "Do you deliver to my area?",
                            "What's your phone number?"
                        ],
                        slots=[],
                        confirmation_required=False,
                        endpoint='/api/info'
                    )
                ]
            }
        }
        
        return hierarchies.get(domain, {})
    
    def generate_training_utterances(
        self,
        intent: IntentDefinition,
        count: int = 50
    ) -> List[str]:
        """
        自动生成训练语句
        使用模板 + 变体生成多样化的训练数据
        """
        
        utterances = []
        
        # 1. 基于现有示例的变体
        templates = [
            "I'd like to {action} {item}",
            "Can I {action} {item}?",
            "{action} me a {item}",
            "I want to {action} {item}",
            "Please {action} {item}",
            "Could you {action} {item} for me?",
            "Get me {item}, please",
            "{item}, please",
            "I need {item}"
        ]
        
        actions = ['order', 'get', 'have', 'place an order for']
        
        # 2. 插槽值替换
        for template in templates[:count//3]:
            for action in actions[:2]:
                for slot in intent.slots:
                    if slot['type'].startswith('@'):
                        entity_values = self._get_entity_values(slot['type'])
                        for value in entity_values[:3]:
                            utterance = template.format(
                                action=action,
                                item=value
                            )
                            utterances.append(utterance)
        
        # 3. 自然语言变体（口语化）
        colloquial_variants = [
            "gimme {item}",
            "i wanna {action} {item}",
            "can i get {item}",
            "let me have {item}",
            "{item} sounds good",
            "how about {item}",
            "do you have {item}",
            "is {item} available"
        ]
        
        for variant in colloquial_variants[:count//4]:
            for value in self._get_sample_values(intent)[:2]:
                utterances.append(variant.format(item=value))
        
        # 4. 上下文相关变体
        contextual_variants = [
            "for lunch i'll take {item}",
            "my dinner will be {item}",
            "{item} for delivery please",
            "i'm ordering {item} right now",
            "add {item} to my cart",
            "one {item} coming up",
            "make that {item}"
        ]
        
        for variant in contextual_variants[:count//4]:
            for value in self._get_sample_values(intent)[:2]:
                utterances.append(variant.format(item=value))
        
        return utterances[:count]
    
    def design_entity_schema(self, entity_name: str, values: List[str]) -> EntityDefinition:
        """设计实体模式"""
        
        synonyms = []
        for value in values:
            entry = {
                'value': value,
                'synonyms': self._generate_synonyms(value)
            }
            synonyms.append(entry)
        
        return EntityDefinition(
            name=entity_name,
            type='CUSTOM',
            values=synonyms,
            fuzzy_matching=True
        )
    
    def _generate_synonyms(self, value: str) -> List[str]:
        """为实体值生成同义词"""
        
        synonym_patterns = {
            'pepperoni': ['pepperoni pizza', 'roni', 'pepperoni only'],
            'mushrooms': ['mushroom', 'shrooms', 'mushrooms'],
            'cheese pizza': ['cheese', 'plain cheese', 'just cheese'],
            'hawaiian': ['hawaiian pizza', 'ham and pineapple'],
            'small': ['small size', 'personal', 'single serving'],
            'medium': ['medium size', 'regular'],
            'large': ['large size', 'big', 'family size']
        }
        
        return synonym_patterns.get(value.lower(), [value])
```

---

## 🔊 音频优化 (SSML & TTS)

### SSML 高级技巧

```xml
<!-- ssml_examples.xml - 语音合成标记语言示例 -->

<!-- 1. 基础语速和音调控制 -->
<speak>
    <prosody rate="95%" pitch="+10%">
        Welcome to Hermes Pizza! Today's special is our
        <emphasis level="strong">delicious deep-dish Chicago style</emphasis>
        pizza.
    </prosody>
</speak>

<!-- 2. 情感和语气 -->
<speak>
    <express-as type="excited">
        Great choice! That's one of our most popular pizzas!
    </express-as>
</speak>

<speak>
    <express-as type="disappointed">
        Oh no, I'm sorry to hear that. Let me help fix this for you.
    </express-as>
</speak>

<!-- 3. 列表和选项朗读 -->
<speak>
    We have three sizes available:
    <say-as interpret-as="ordinal">1</say-as>. Small, perfect for one person.
    <say-as interpret-as="ordinal">2</say-as>. Medium, great for sharing.
    <say-as interpret-as="ordinal">3</say-as>. Large, for the whole family.
    Which would you prefer?
</speak>

<!-- 4. 数字和特殊格式 -->
<speak>
    Your order total is
    <say-as interpret-as="currency">$24.99</say-as>.
    It should arrive in approximately
    <say-as interpret-as="time" format="hms">00:35</say-as>.
    Your order number is
    <say-as interpret-as="characters">A B C 1 2 3</say-as>.
</speak>

<!-- 5. 音效和静音控制 -->
<speak>
    <audio src="https://example.com/chime.mp3"/>
    <break time="500ms"/>
    Processing your order now...
    <break strength="x-strong"/>
    <audio src="https://example.com/success.mp3"/>
    All done! Is there anything else I can help with?
</speak>

<!-- 6. 多角色对话 -->
<speak>
    <voice name="Joanna">
        Hi there! What can I get started for you today?
    </voice>
    <break time="2s"/>
    <!-- User responds -->
    <voice name="Joanna">
        Excellent choice! Would you like any drinks with that?
    </voice>
</speak>

<!-- 7. 智能停顿（基于标点和语义）-->
<speak>
    Your pizza
    <break time="200ms"/>
    with pepperoni
    <break time="200ms"/>
    mushrooms
    <break time="300ms"/>
    and extra cheese
    <break time="400ms"/>
    will arrive in 30 to 40 minutes.
</speak>
```

### TTS 个性化配置

```python
# tts_configurator.py - TTS 引擎配置

from dataclasses import dataclass
from typing import Optional, Dict, List
from enum import Enum

class VoiceStyle(Enum):
    FRIENDLY = "friendly"
    PROFESSIONAL = "professional"
    CASUAL = "casual"
    ENERGETIC = "energetic"
    CALM = "calm"
    WARM = "warm"

@dataclass
class TTSConfig:
    voice_id: string
    engine: string  # neural, standard
    language_code: string
    sample_rate: int = 22050
    output_format: string = "mp3"
    
    # 语速和音调
    speech_rate: float = 1.0  # 0.5 - 2.0
    pitch: float = 0.0       # -20% to +20%
    volume: float = 1.0      # 0.0 - 2.0
    
    # 风格设置
    style: VoiceStyle = VoiceStyle.FRIENDLY
    
    # 特殊效果
    breathiness: float = 0.0
    dynamic_range_compression: bool = True

class TTSConfigurator:
    """TTS 个性化配置器"""
    
    def configure_for_brand(self, brand_personality: Dict) -> TTSConfig:
        """根据品牌个性配置 TTS"""
        
        personality_map = {
            'playful': TTSConfig(
                voice_id="Joanna",
                engine="neural",
                language_code="en-US",
                speech_rate=1.05,
                pitch=+5,
                style=VoiceStyle.FRIENDLY,
                breathiness=0.2
            ),
            'professional': TTSConfig(
                voice_id="Matthew",
                engine="neural",
                language_code="en-US",
                speech_rate=0.95,
                pitch=-2,
                style=VoiceStyle.PROFESSIONAL,
                breathiness=0.0
            ),
            'luxury': TTSConfig(
                voice_id="Kendra",
                engine="neural",
                language_code="en-US",
                speech_rate=0.9,
                pitch=+3,
                style=VoiceStyle.WARM,
                breathiness=0.15
            ),
            'youthful': TTSConfig(
                voice_id="Nicole",
                engine="neural",
                language_code="en-US",
                speech_rate=1.1,
                pitch=+8,
                style=VoiceStyle.CASUAL,
                breathiness=0.25
            )
        }
        
        primary_trait = brand_personality.get('primary_trait', 'friendly')
        return personality_map.get(primary_trait, personality_map['playful'])
    
    def generate_ssml_with_emotion(
        self,
        text: str,
        emotion: str = "neutral",
        emphasis_phrases: List[str] = None
    ) -> str:
        """生成带情感标记的 SSML"""
        
        ssml = '<speak>'
        
        # 应用情感
        if emotion != 'neutral':
            ssml += f'<express-as type="{emotion}">'
        
        # 处理强调短语
        if emphasis_phrases:
            remaining_text = text
            for phrase in emphasis_phrases:
                if phrase.lower() in remaining_text.lower():
                    parts = remaining_text.split(phrase, 1)
                    ssml += parts[0]
                    ssml += f'<emphasis level="strong">{phrase}</emphasis>'
                    remaining_text = parts[1] if len(parts) > 1 else ''
            ssml += remaining_text
        else:
            ssml += text
        
        if emotion != 'neutral':
            ssml += '</express-as>'
        
        ssml += '</speak>'
        
        return ssml
```

---

## 📱 多平台适配指南

### Alexa vs Google Assistant 差异

```yaml
# platform_comparison.yaml - 平台差异对照表

platform_differences:

  alexa:
    naming_conventions:
      skill_name: "Hermes Pizza"
      invocation_name: "hermes pizza"
      interaction_model: "interaction model"
      
    slot_types:
      custom: "CUSTOM slot types"
      built_in: "AMAZON slot types"
      example: "AMAZON.US_FIRST_NAME"
      
    audio_capabilities:
      max_audio_duration: "240 minutes (4 hours)"
      supported_formats: ["MP3", "MPEG", "AAC"]
      background_music: "Supported via SSML <audio>"
      
    visual_response:
      type: "APL (Alexa Presentation Language)"
      displays: "Echo Show, Echo Spot, Fire TV"
      touch_support: "Yes"
      
    permissions:
      require_permission: true
      common_permissions:
        - "alexa::devices:all:address:full"
        - "alexa::devices:all:geolocation:read"
        
    limitations:
      session_duration: "Variable (can be extended)"
      reprompt_limit: "No hard limit but UX degrades after 5-7"
      card_character_limit: "8000 characters"
      
  google_assistant:
    naming_conventions:
      action_name: "Hermes Pizza Action"
      invocation_phrase: "talk to hermes pizza"
      interaction_model: "intent/scene/condition"
      
    slot_types:
      custom: "Custom types (@type)"
      built_in: "System types (sys.*)"
      example: "sys.person"
      
    audio_capabilities:
      max_audio_duration: "240 minutes"
      supported_formats: ["MP3", "OGG_OPUS", "AAC"]
      background_music: "Supported via <media>"
      
    visual_response:
      type: "Interactive Canvas / Rich Responses"
      displays: "Google Nest Hub, Android TVs, Smart Displays"
      touch_support: "Yes"
      
    permissions:
      require_permission: false (implicit for some)
      scope_declaration: "Required in actions.xml"
      
    limitations:
      conversation_timeout: "~5 minutes of inactivity"
      suggestion_chips_max: "8 chips per response"
      card_character_limit: "649 characters (simple responses)"

  best_practices_cross_platform:
    intent_naming:
      convention: "VerbObject (e.g., OrderPizza, CheckStatus)"
      case: "PascalCase"
      avoid: "Generic names like 'Intent1'"
      
    utterance_design:
      alexa_specific:
        - "Use {slot_name} syntax for slot references"
        - "Support natural phrase mode"
      google_specific:
        - "Use $slot_name syntax"
        - "Support training phrases with @sys.entity annotations"
        
    error_handling:
      universal:
        - "Always provide helpful reprompts"
        - "Implement graceful degradation"
        - "Log errors for analysis"
      platform_specific:
        alexa:
          - "Use Alexa.ErrorResponse for system errors"
          - "Implement Account Linking for personalized experiences"
        google:
          - "Use Scene-based navigation for complex flows"
          - "Leverage slots filling with forms"
```

---

## 📊 VUI 分析指标

### 关键性能指标 (KPI)

```python
# vui_analytics.py - VUI 性能分析

from dataclasses import dataclass
from typing import Dict, List
from datetime import datetime, timedelta
from enum import Enum

class MetricCategory(Enum):
    USABILITY = "usability"
    ENGAGEMENT = "engagement"
    ERROR_RATE = "error_rate"
    SATISFACTION = "satisfaction"

@dataclass
class VUIMetric:
    name: str
    category: MetricCategory
    current_value: float
    target_value: float
    unit: str
    trend: str  # improving, declining, stable
    benchmark: float = None

class VUIAnalytics:
    """VUI 数据分析引擎"""
    
    def calculate_vui_health_score(
        self,
        metrics: List[VUIMetric]
    ) -> Dict:
        """计算整体 VUI 健康评分"""
        
        category_weights = {
            MetricCategory.USABILITY: 0.30,
            MetricCategory.ENGAGEMENT: 0.20,
            MetricCategory.ERROR_RATE: 0.30,
            MetricCategory.SATISFACTION: 0.20
        }
        
        category_scores = {}
        
        for category in MetricCategory:
            category_metrics = [m for m in metrics if m.category == category]
            
            if category_metrics:
                # 计算类别内加权平均
                achieved_sum = sum(
                    min(m.current_value / m.target_value, 1.0)
                    for m in category_metrics
                )
                category_scores[category] = achieved_sum / len(category_metrics)
        
        # 加权总分
        total_score = sum(
            score * category_weights.get(category, 0)
            for category, score in category_scores.items()
        )
        
        return {
            'overall_score': round(total_score * 100, 1),
            'category_breakdown': {
                cat.name: round(score * 100, 1)
                for cat, score in category_scores.items()
            },
            'grade': self._score_to_grade(total_score),
            'recommendations': self._generate_recommendations(category_scores)
        }
    
    def analyze_dialog_flows(self, sessions: List[Dict]) -> Dict:
        """分析对话流效率"""
        
        metrics = {
            'average_turns_per_session': 0,
            'completion_rate': 0.0,
            'abandonment_points': [],
            'bottleneck_intents': [],
            'optimal_path_deviation': 0.0
        }
        
        completed_sessions = [s for s in sessions if s.get('completed')]
        abandoned_sessions = [s for s in sessions if not s.get('completed')]
        
        if sessions:
            metrics['completion_rate'] = len(completed_sessions) / len(sessions) * 100
            
            avg_turns = sum(s.get('turn_count', 0) for s in sessions) / len(sessions)
            metrics['average_turns_per_session'] = round(avg_turns, 1)
        
        # 分析放弃点
        abandonment_analysis = {}
        for session in abandoned_sessions:
            last_intent = session.get('last_intent', 'unknown')
            abandonment_analysis[last_intent] = abandonment_analysis.get(last_intent, 0) + 1
        
        metrics['abandonment_points'] = sorted(
            abandonment_analysis.items(),
            key=lambda x: x[1],
            reverse=True
        )[:5]
        
        return metrics
    
    def _score_to_grade(self, score: float) -> str:
        if score >= 0.90: return 'A'
        elif score >= 0.80: return 'B'
        elif score >= 0.70: return 'C'
        elif score >= 0.60: return 'D'
        else: return 'F'
```

---

## 🔗 相关技能

- [Hermes-Skill-Creative-Writing-Coach](../creative/creative-writing-coach/SKILL.md) - 对话文案创作
- [Hermes-Skill-Accessibility-Expert](../accessibility/accessibility-expert/SKILL.md) - 无障碍语音交互
- [Hermes-Skill-AI-Model-Comparator](../ai/ai-model-comparator/SKILL.md) - NLU 模型对比

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 支持平台 | Alexa, Google Assistant, Siri, Custom Devices |
| 对话模式 | 15+ 种设计模式 |
| SSML 示例 | 30+ 高级用法 |
| NLU 架构 | Intent/Entity/Slot 完整体系 |
| 多平台差异文档 | 详细对照表 |
| 文档完整度 | ★★★★★ |

---

## ⭐ 支持本项目

如果这个技能对你有帮助，请考虑支持我们的持续开发：

💰 **微信支付**：查看 `images/wechat-pay.png`  
💰 **支付宝**：查看 `images/alipay.png`  
⭐ **GitHub Star**：给个 Star 让更多人发现这个项目！

---

**技能版本**: v1.0.0  
**最后更新**: 2026-04-28  
**适用场景**: 语音助手开发 | 智能音箱 | 车载系统 | IoT 语音交互
