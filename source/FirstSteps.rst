================================
Write your first Hello-World-App
================================

Prepare a workspace
-------------------
To get started you need to create a folder for your app andd add a CMakeLists.txt file into it:

::

   INCLUDE_DIRECTORIES(
     ${PROJECT_SOURCE_DIR}
     ../
   )

    SET ( 00_helloworld_src
      00_HelloWorld/HelloWorld.cpp
	    00_HelloWorld/README.md
    )

    ADD_EXECUTABLE( HelloWorld
      ${00_helloworld_src}
    )

    link_directories(
      ${CMAKE_CURRENT_SOURCE_DIR}/../../ThirdParty/glew/Debug
      ${CMAKE_CURRENT_SOURCE_DIR}/../../ThirdParty/glew/Release
    )

    target_link_libraries ( HelloWorld   osre )

::

Prepare the code
----------------
Here the code:

::

    #include <osre/App/App.h>
    #include <osre/Common/Logger.h>
    #include <osre/RenderBackend/RenderBackendService.h>
    #include <osre/Scene/MeshBuilder.h>
    #include <osre/Scene/Camera.h>
    #include <osre/Platform/AbstractWindow.h>
    #include <glm/gtc/matrix_transform.hpp>
    #include <glm/gtc/type_ptr.hpp>

    using namespace ::OSRE;
    using namespace ::OSRE::App;
    using namespace ::OSRE::RenderBackend;

    // To identify local log entries we will define this tag.
    static const c8 *Tag = "HelloWorldApp";

    ///
    /// The example application, will create the render environment and render a simple triangle onto it
    ///
    class HelloWorldApp : public AppBase {
        /// The transform block, contains the model-, view- and projection-matrix
        TransformMatrixBlock m_transformMatrix;
        /// The entity to render
        Entity *mEntity;
        /// The keyboard controller to rotate the triangle
        Scene::AnimationControllerBase *mKeyboardTransCtrl;

    public:
        /// The class constructor with the incoming arguments from the command line.
        HelloWorldApp(int argc, char *argv[]) :
                AppBase(argc, (const char **)argv),
                m_transformMatrix(),
                mEntity(nullptr),
                mKeyboardTransCtrl(nullptr) {
            // empty
        }

        /// The class destructor.
        ~HelloWorldApp() override {
            // empty
        }

    protected:
        bool onCreate() override {
            if (!AppBase::onCreate()) {
	        osre_error(Tag, "Error while creating application basics.");
                return false;
            }

            AppBase::setWindowsTitle("Hello-World sample! Rotate with keyboard: w, a, s, d, scroll with q, e");
            World *world = getActiveWorld();
            mEntity = new Entity("entity", *AppBase::getIdContainer(), world);
            Scene::Camera *camera = world->addCamera("camera_1");
            ui32 w, h;
            AppBase::getResolution(w, h);        
            camera->setProjectionParameters(60.f, (f32)w, (f32)h, 0.001f, 1000.f);

            Scene::MeshBuilder meshBuilder;
            RenderBackend::Mesh *mesh = meshBuilder.allocTriangles(VertexType::ColorVertex, BufferAccessType::ReadOnly).getMesh();
            if (nullptr != mesh) {
                mEntity->addStaticMesh(mesh);
                world->addEntity(mEntity);            
                camera->observeBoundingBox(mEntity->getAABB());
            }
            mKeyboardTransCtrl = AppBase::getTransformController(DefaultControllerType::KeyboardCtrl, m_transformMatrix);

            return true;
        }

        void onUpdate() override {
            RenderBackendService *rbSrv = getRenderBackendService();
            mKeyboardTransCtrl->update(rbSrv);

            rbSrv->beginPass(PipelinePass::getPassNameById(RenderPassId));
            rbSrv->beginRenderBatch("b1");

            rbSrv->setMatrix(MatrixType::Model, m_transformMatrix.m_model);

            rbSrv->endRenderBatch();
            rbSrv->endPass();

            AppBase::onUpdate();
        }
    };

    /// Will generate the main function.
    OSRE_MAIN(HelloWorldApp)

::
