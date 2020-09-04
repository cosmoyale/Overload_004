

Setup:Use Logging Queue from previous paperPEQ seeds should be saved and used for non-PEQ use cases (to guarantee same distribution) (apples to apples)

Logic: 
Init vector of shapesDraw (Iterate vector)Update positionsDraw (Iterate vector)Goto 3

Suitability of this patternLarge degree of variance of object size will waste space.
Method DispatchEach struct and union implements same method with union using switch/case in every method.Dispatch Method
What other patterns do we want to compareCRTPvanilla C++ virtualVector of same data structure (baseline)N vectors of static types1 vector per type

Testing ScenariosSingle random seed chosen as typical use caseMany seeds tested and statistical analysis provided

Stats gatheredMemory usage (loss of efficiency due to union of varying types)Typical latency stats
Use Cases: 
Use casesVector of dynamic objectsQueue between threadscritical -> non-criticalnon-critical -> criticalcritical -> criticalnon-critical -> non-criticalReally need for this complexity when performance is not a concern?Need some further explorationCode: 
class DescDispatch{public: template <class T> static auto dispatch(T& x) {return x.desc;}};
class DrawDispatch{public: template <class T> static auto dispatch(T& x) {return x.Draw();}};
....
std::cout << "DD1 " << c.Dispatch<DescDispatch>() << std::endl; std::cout << "DD2 " << s.Dispatch<DescDispatch>() << std::endl; std::cout << "DD3 " << t.Dispatch<DescDispatch>() << std::endl; std::cout << "DD4 " << o.Dispatch<DescDispatch>() << std::endl;
c.Dispatch<DrawDispatch>();
 template <class Dispatcher> auto Dispatch() { switch(m_st) { case ShapeType::Square: { auto &x = get_by_type<Square>(); return Dispatcher::dispatch(x); break; } case ShapeType::Triangle: { auto &x = get_by_type<Triangle>(); return Dispatcher::dispatch(x); break; } case ShapeType::Circle: { auto &x = get_by_type<Circle>(); return Dispatcher::dispatch(x); break; } case ShapeType::Octagon: { auto &x = get_by_type<Octagon>(); return Dispatcher::dispatch(x); break; } default: std::cout << "Unknown Shape" << std::endl; break; }
  
